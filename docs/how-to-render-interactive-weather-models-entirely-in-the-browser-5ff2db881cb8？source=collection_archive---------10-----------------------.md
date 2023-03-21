# 如何完全在浏览器中呈现交互式天气模型

> 原文：<https://towardsdatascience.com/how-to-render-interactive-weather-models-entirely-in-the-browser-5ff2db881cb8?source=collection_archive---------10----------------------->

![](img/5a90765a0bbe3367a685f7699ca19572.png)

WPC 概率冬季降水指导 72 小时降雪模型，在浏览器中渲染并添加到传单地图。

## 完全使用 JavaScript 将多维地理空间栅格渲染到 web 地图上的技术和用例。

气象爱好者、气象学家和风暴追逐者都知道网络上有无数的气象模型可视化网站。这些平台允许你查看各种参数、级别和几十种不同天气模型的预报时间。例如，当准备一天的风暴追逐时，追逐者可能会查看某个区域的几个参数-早上的露点、当天晚些时候的海角以及下午的 HRRR 模拟反射率。通常提供菜单、按钮和滑块来切换预测时间、模型和参数。

![](img/7cbe63dd3179a9fc2982fb47ef03b074.png)

北美模型在离地面两米处的温度，使用[杜佩奇学院的模型观察器。](https://weather.cod.edu/forecast/)

通常，所有这些天气模型查看器都是用户访问预渲染的非空间天气模型的简单方式，尽管原始数据实际上是地理空间数据。尽管界面不同，但天气模型查看器中的许多内容是一致的:

*   地理区域是固定的。您只能查看预设区域，例如城市、州或地区列表。输出图像的大小也是固定的。一些查看器允许您放大某些区域并等待新的渲染。
*   地理图层和信息稀缺或不存在。在天气模型图像上，您可能只有道路或县是可见的，这可能会使特定区域难以精确定位。
*   许多提供者拥有通常不可查询的查看器，这意味着要获得任何给定点的实际值，需要将数据的颜色与图例进行比较。
*   模型层组合和配色方案是静态的。例如，您不能在另一个变量上叠加特定高度的风流线，除非提供商提供了该组合。

![](img/2fabdcd8f8dad34f478bfe73d6d5962e.png)

[TropicalTidbit 的](https://tropicaltidbits.com/analysis/models/)模型查看器。不想让地图上的所有图层同时出现？太糟糕了。

尽管许多提供者已经对数据的呈现进行了认真的考虑和思考，但是上述缺点在某些情况下可能是显著的和恶化的。例如，考虑到降雪量在几英里范围内可能会有几十英寸的差异，如果您只有县边界来确定自己的方位，评估山区的预测降雪量可能会非常具有挑战性。

考虑下图:

![](img/5ffe83b85887f385988b9f46dfc38f7a.png)

即使作为一个拥有地理学位的预测者，我也很难从这张图表中获得有用的数据。看看科罗拉多州:

1.  汽船和北科罗拉多黄等带的关系在哪里？它是蓝色的还是绿色的？(这将是 2 英寸和 6 英寸雪的区别)。
2.  在 I-70 上，从韦尔山口以东到以西的降雪量分布如何变化？
3.  戈尔山脉和它南边的十英里山脉相比，降雪量有多少？

如果你在科罗拉多州做了足够多的预测，你最终会记住精确到英里的县轮廓与山脉的关系，因为当你在许多模型查看器上查看整个州的天气分布时，这通常是你必须做的全部工作。

也就是说，提到这些平台的好处是很重要的——最明显的是使用预先渲染的静态图像。这些都很快，很容易在几秒钟内加载一个模型的所有预测小时数，并且您通常可以很容易地让服务器生成数据的动画 GIF。固定区域防止了数据的“误用”——例如放大到超出天气模型适用范围的分辨率。这两个因素允许一致的天气模型可视化，如果您正在下载图像并将其存档以供比较或重新分发，这将很有帮助。

![](img/c932da14fa894ef03922a9c38c62300b.png)

在杜佩奇学院的浏览器上下载整个模型的预测既快又容易。

# 构建您自己的模型可视化工具—概述

在上一篇文章中，我讨论了如何[构建和自动化自己的天气模型可视化工具](https://medium.com/swlh/automated-weather-model-processing-with-foss4g-lessons-learned-8aaaeda1e3bc)。这项技术有一个有趣的变化，尽管服务器仍在渲染数据，但它将数据作为地理空间层([OGC·WMS](https://www.opengeospatial.org/standards/wms))提供服务，可以添加到交互式 web 地图中。这在预渲染静态图像的性能和完全地理空间数据的交互性之间取得了良好的平衡。我将向您展示如何更进一步，完全在浏览器中进行渲染，不需要服务器！*根据我上面的链接，这篇文章将假设你有 GeoTIFF 格式的天气模型，并投影到 EPSG:4326。只需一个* [*GDAL*](https://gdal.org/) *命令，即可将原始天气模型数据(GRIB2 格式)转换为 GeoTIFF。*

使用 web 浏览器在客户端呈现天气模型数据的缺点很多，但是这种技术有一些潜在的使用案例。2020 年的 Web 开发似乎已经足够成熟和强大，可以让它成为一个实际可行的提议。以下是我们将会遇到的主要问题:

*   缓慢。JavaScript 并不是将原始多维栅格转换成 RGB 图像或矢量等值线的理想语言。
*   数据使用。原始天气模型数据不像简单的 JPEG 那样紧凑。
*   兼容性。我们编写的最终代码无法在 Internet Explorer 中运行，在较旧的硬件上运行起来也很糟糕。

相比之下，以下是我们将享受的一些好处:

*   在完全交互式的 web 地图上呈现数据的能力，这意味着我们可以实时添加任意数量的地理数据层(或其他模型参数),并按照我们想要的方式设置样式。
*   可以直接访问数据，例如查询原始模型数据的点或区域。这不仅仅是单击地图并获取某一点的数据，一个用例可能是定义一个位置列表或一组点，以表格格式显示数据或在地图上叠加数据。
*   以我们选择的任何方式渲染天气模型数据或数据组合的能力，如栅格、等值线、等值线(等值线/等高线)、流线等。渲染、样式(例如，轮廓的宽度)和色带都可以由用户修改(如果需要),并即时更新可视化。各种模型甚至可以在数学上组合起来，以产生特定的可视化效果，而不是必须提前设置自动化处理脚本来实现。

我们也有一些策略来缓解一些棘手问题:

*   使用 [web workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers) 来处理和渲染地理标记。这将把密集的计算分散到用户 CPU 的各个核心上。
*   使用[云优化地理标签](https://www.cogeo.org/) (COGs)或 [WCS 标准](https://mapserver.org/ogc/wcs_format.html)将原始数据传输给用户，仅限于他们感兴趣的区域和范围。此外，对土工格栅施加适当的压缩可以使它们减小到合理的尺寸。

记住这几点，你能想出一些可以享受客户端渲染好处的用例吗？对我来说，可视化的质量立即浮现在脑海中。我曾经使用定制的 QGIS 项目来导出带有天气模型数据的漂亮地图，但是仍然需要手工劳动。借助一个外观精美的 web 浏览器可视化平台，我可以快速地对多个模型、参数和预报时间进行分析，同时只需点击一下鼠标，就可以导出天气模型图形和地图进行重新分发。

在我之前提到的关于使用 MapServer 分发天气模型数据的文章中，我也遇到了一些具体的问题。这些问题都与可视化产品有关，在这些产品中，图形被从数据本身中大量移除，需要将各种栅格渲染为最终的 RGB 图像(例如，风速栅格上的流线或风倒钩)，这与我打算在 MapServer 中采用的比例和样式不可知的模型数据分布策略模型不兼容。然而，当在 web 浏览器中处理时，这些问题很容易解决:

*   天气模型组合计算。几个流行的天气模型可视化是不可能的，简单地分发转换后的数据，而不渲染一个 RGB 图像。最好的例子是*复合反射率+降水类型*，它显示了您可能期望的模拟雷达——雨显示为绿色-黄色-红色的典型色阶，而雪显示为蓝色阴影。这实际上需要至少三个不同天气模型参数栅格的组合，或者如果要处理所有降水类型(冰粒和冻雨)，则需要五个。据我所知，没有办法在 MapServer 中即时完成这项工作，因为需要根据降水类型使用不同的色标，并组合成最终的图像。一个更简单的例子是，大多数模型中的风向不是单个参数，而是两个栅格(水平和垂直矢量分量)，需要结合三角学来计算实际方向和速度。

![](img/995e431cbef3789db5727eb316a1dab2.png)

降水类型，由杜佩奇学院渲染。

*   风的流线。MapServer 能够根据栅格生成等值线，甚至可以生成穿过栅格字段的方向箭头，但流线是不可能的。然而，许多受欢迎的产品依靠流线来显示像中层大气风这样的东西。

![](img/f595cf423b98ff0bb10c8a40f9b66f61.png)

流线在 weather.us 的渲染中很明显。

在上面的例子中，你可能已经注意到了一件事，那就是尽管可视化效果很好，但看起来并不那么好。图像很小，数据也很狭窄，这对预测者来说并不容易。当重新发布给天气预报读者时，它们甚至更麻烦，这些读者可能不是数据极客，只是想知道他们下周末应该去哪里滑雪。我想人们可能会承认，他们 90 年代后期的像素化审美*确实给人一种纯粹的数据权威的感觉，除了传达原始数据，他们当然不会浪费任何时间做任何事情。*

# *构建您自己的模型可视化工具——在实践中*

*让我们来看看天气模型完全在 web 浏览器中呈现时会是什么样子。这些是从我的网站上摘下来的，用在了[我的预测](https://medium.com/14ers-forecast)中。首先，这是我用 Angular 为它建立的界面:*

*![](img/e39c8c646a27afce399f8443f987b5fd.png)*

*下半部分被切掉，以隐藏我在设计中尚未有效实现的按钮墓地:)*

*一些示例输出图像:*

*![](img/f52aa1592d0df1bf88ad2869e680d2a9.png)*

*现在，让我们放大交互式地图，并启用其他一些层。当我说可调整时，我的意思是这可以由用户在网页上实时完成:*

*![](img/8098c95c6db0a4007bcc0db4394da0df.png)*

*有流线的图层怎么样？*

*![](img/6d08c6efbeae11597c3eb0ea6ab71432.png)*

*是的，流线型箭头在工具提示的顶部。我会修好的。*

*我们甚至有可怕的*复合反射率+类型*层工作正常。你可以看到落基山脉上空的雪是深蓝色的，内布拉斯加州的小雨是灰色的(嗯，很抱歉有点误导人的浅蓝色)。这都是在浏览器中计算出来的。*

*![](img/0e5a3ce0007c983e15fac163a820b374.png)*

*在我住的地方没有太多有趣的天气，这是我唯一能找到的两种降水类型的框架。*

*以下是完成客户端天气模型渲染所需的主要步骤，前提是您已经准备好了 web 地图。我使用传单，但最终需要扩展它这么多，如果你是从零开始，OpenLayers 可能更适合。*

1.  *`Fetch`适当的模型、时间戳、参数、级别和预报小时的 GeoTIFF 数据，并将流作为`ArrayBuffer`读取。*
2.  *解析`ArrayBuffer`以获得地理信息和原始数据的嵌套数组。Geotiff.js 是这方面的首选库，尽管它不能很好地与 Angular 配合使用，除非您导入缩小版。您需要缓存这些数据，以防需要对其进行查询或重新呈现可视化效果。*
3.  *提取适当波段的数据(对于多维数据，例如一个 TIFF 中的多个预测小时或一个中的多个参数),并使用 web workers 对嵌套数据数组运行[行进正方形算法](https://en.wikipedia.org/wiki/Marching_squares),您可以使用该算法生成等值线(多边形)或等值线(折线),具体取决于您是否想要填充/着色数据或等值线。你可以使用像[光栅行进正方形](https://github.com/rveciana/raster-marching-squares)这样的库。对于流线，可以使用[光栅流线](https://github.com/rveciana/raster-streamlines)库。**这是计算量最大的一步。**一旦计算完成，您将需要缓存所有这些数据。*
4.  *在创建等值线和等值线时，对其应用必要的地理变换，然后将其提供给 web 地图。例如，在传单中，我将这些数据作为一个`L.GeoJSON`层。确保为每个波段或线条提供数据值，以便可以适当地设置样式。把它添加到地图上。*
5.  *添加其他功能，如点查询和可视化下载(“另存为 JPEG”)。*

*让我们深入研究一下细节吧！我将直接从我的代码库中使用 TypeScript 和 ES6 语法，它使用 Angular 框架。*

# *1.检索地理信息数据*

*这是最简单的部分。一些策略可能是:*

*   *只需从网络服务器下载文件。*
*   *创建一个 API，为模型、参数、预测时间、地理区域等检索适当的文件。*
*   *✔️使用云优化地理标志*
*   *使用 MapServer 或其他地理数据服务器处理 WCS 请求，并将数据作为 GeoTIFF 返回。*

*想想你希望你的地理结构是怎样的。为了节省数据，您可能希望为每个模型、参数、级别、预测时间等返回一个 TIFF。但是，如果客户希望预加载所有内容(例如，使用滑块查看所有预测小时数)，您可以将多个预测小时数存储为一个 TIFF 中的几个区域。这将创建一个初始的，较长的下载时间，但会稍微加快整体解析时间。*

*不要忘记压缩。虽然你可以对 TIFFs 使用 JPEG 压缩，把它们的文件大小压缩到和 JPEG 差不多，但是这不是对数据的负责任的使用。这些不是图像，而是原始数据的数组，在渲染之前应该尊重其完整性。另外，我不认为`geotiff.js`库能够处理这种压缩方法。我发现高水平(zlevel = 9)的`deflate`在不严重影响解析性能的情况下减少了 50%的大小。*

*您可能会用`fetch`或使用`geotiff.js`的内置函数来检索 GeoTIFF。该库可以[原生读取云优化的 GeoTIFFs！](https://geoexamples.com/other/2019/02/08/cog-tutorial.html#the-cloud-optimized-geotiff-format)如果使用常规的 GeoTIFF，您需要将`fetch`请求流(如果您创建了一个)作为`ArrayBuffer`读取:*

```
*const response = await fetch( <the url of your tiff> );
const arrayBuffer = await response.arrayBuffer();*
```

# *2.解析 GeoTIFF 数据*

*按照上面的例子，我们可以使用`geotiff.js`将`ArrayBuffer`解析成一个`GeoTiff`对象。我们可以从该对象中检索实际的图像数据本身:*

```
*const geoTiff = await fromArrayBuffer( arrayBuffer );
const geoTiffImage = await geoTiff.getImage();*
```

*不要忘记获取图像元数据和地理数据，我们稍后会用到它们:*

```
*const tiffWidth = geoTiffImage.getWidth();
const tiffHeight = geoTiffImage.getHeight();
const tiepoint = geoTiffImage.getTiePoints()[ 0 ];
const pixelScale = geoTiffImage.getFileDirectory().ModelPixelScale;
const geoTransform = [ 
    tiepoint.x, 
    pixelScale[ 0 ], 
    0, 
    tiepoint.y, 
    0, 
    -1 * pixelScale[ 1 ] 
];*
```

*如果图像中有多个波段，您可以轻松获得它们的确切数量:*

```
*const bands = await geoTiffImage.getSamplesPerPixel();*
```

# *3.根据数据构建等值线/等值线/流线*

*除了风倒刺(这对天气预报员来说可能很好，但对普通公众来说几乎没用)，除了等深线、等值线或流线，我想不出任何其他方法来呈现天气模型数据。等深线是属于特定数据范围的多边形。您可以使用它们来生成彩色的温度或积雪地图。等值线是在数据区域边界绘制的等高线。流线有助于可视化既有速度又有方向的数据。这些都可以创建为矢量并添加到地图上。*

*我们需要破解 GeoTIFF 数据来构建我们的地理向量。您也可以将它作为图像渲染到画布上，但我不会在这里介绍(参见致谢部分)。*

*获取适当波段的图像数据(其中`i`是波段号):*

```
*const bandData = await geoTiffImage.readRasters({ samples: [ i ] });*
```

*构建原始数据的嵌套数组:*

```
*const dataArr = new Array( tiffHeight );
for ( let i = 0; i < tiffHeight; i++ ) {
    dataArr[ i ] = new Array( tiffWidth );
    for ( let ii = 0; ii < tiffWidth; ii++ ) {
        dataArr[ i ][ ii ] = bandData[ 0 ][ ii + i * tiffWidth ];
    }
}*
```

*我们将使用`raster-marching-squares`库来生成我们的特性，但是出于性能考虑，我想作为一个 web worker 来运行它，这样用户界面就不会锁定。如果你正在使用 Angular，运行`ng generate web-worker`就可以很容易地用 Angular-CLI 设置一个 web worker。*

*在 web worker 内部，我们将把`raster-marching-squares`作为`rastertools`导入:*

```
*import * as rastertools from 'raster-marching-squares';*
```

*这将等深线和等值线渲染为`rastertools.isobands()`或`rastertools.isolines()`。这两个函数都需要以下参数:*

*   *嵌套数据数组(`dataArr`)*
*   *光栅的投影(`geoTransform`)*
*   *数据的中断/间隔时间。这些间隔将始终出现在这些函数的输出中，而不管这些函数中实际上是否有任何特征。例如，如果您将`[-5,0,5]`作为温度模型的区间，您将会收到三条与-5 度、0 度和 5 度相关的等深线或等值线。如果天气模型具有这些范围内的数据，那么这些等值线或等深线将具有包含这些区域的几何图形。否则，它们将是空特征。*

*我们最终会想要设计等值线的样式或在等值线上放置标签，所以我做的一件事是为我想要可视化的所有各种参数设置色标，为特定值传递一种颜色。然后，我设置了一个函数，它将从色标中产生固定数量的间隔(比如 100——平滑，但需要一段时间来计算),并使用`tinygradient`来产生用于图例的色标的 CSS 表示。例如，积雪的色标如下所示:*

```
*"SnowAccumM": [
    { value: 1.8288, color: "#ffcc01" },
    { value: 1.2192, color: "#ff4501" },
    { value: 0.6096, color: "#e51298" },
    { value: 0.3048, color: "#9f0ded" },
    { value: 0.0762, color: "#5993ff" },
    { value: 0.00254, color: "#d6dadd" },
    { value: 0, color: "#ffffff" }
],*
```

*interval 创建函数也用`Number.MAX_SAFE_INTEGER`和它的负等价物来限定 intervals 数组。如果做得好，并在其中加入一些单位转换，就可以在一个紧凑的类中处理 isoband 的创建、样式和图例。*

*![](img/775167fafc1c89dcce37d376beb3e521.png)*

*积雪的等深线，以米为单位，转换成英寸。这有 100 个间隔，其中大部分没有被使用，因此没有被优化。希望很快会有一场用掉所有 100 个间隔的暴风雪…*

*运行等值线或等值线函数的工作线程将监听三个必要的参数。我们还将传入一个`type`参数来告诉它使用哪个函数:*

```
*addEventListener( 'message', ( { data } ) => {
    try {
        const features = rastertools[type]( 
            data.data, 
            data.geoTransform, 
            data.intervals 
        );
        postMessage ( { features } );
    }
    catch ( err ) {
        postMessage( { error: err.message } );
    }
}*
```

*该函数被包装在一个`try...catch`中，这是一个好主意，但在实践中没有用，因为当函数出错时，它往往会陷入一个`do...while`循环中，直到内存耗尽。这可能是由于我一天转换的数千个天气模型栅格中的一些被破坏了，我需要修复这些栅格，但是…有人请求吗？*

*现在我们可以调用 worker，向它传递必要的参数，并等待响应。您不必使用`Promise`，它只适合我的特定工作流程:*

```
*const type = 'isobands';const rastertoolsWorker = new Worker(
    `../workers/rastertools.worker`,     // the worker class ☭
    { type: `module` } 
);new Promise( ( resolve, reject ) => {
    rastertoolsWorker.postMessage( { 
        dataArr, 
        geoTransform, 
        intervals,      // array of numbers
        type 
    } ); rastertoolsWorker.addEventListener( 
        'message', 
        event => resolve( event.data )
    );
} ).then (...)          // the isobands are generated!*
```

*如果你做 100 次间歇，这可能需要一点时间。你的笔记本电脑粉丝可能会以惊人的凶猛速度踢你。*

*![](img/cd529c56c50c77808629c95392fcd506.png)*

*在 web 浏览器中从栅格生成矢量？多么明智的想法。这很好。*

*注意，我没有提到流线。如果您使用`raster-streamlines`库，这个计算会非常快，并且可能不需要放在 web worker 中。*

# *4.把它放在地图上*

*如果你用了`raster-marching-squares`，这部分就全给你搞定了。基本上，矢量要素是从栅格中生成的，需要与这些像素的实际地理位置联系起来。我们可以使用由`geotiff.js`提供的`geoTransform`来获取转换信息(希望您的 GeoTIFF 最初位于 EPSG:4326)。我们还需要 GeoJSON 形式的信息，这是一个不可知的几何集合。同样，库提供了这个集合，我们只需要用`L.geoJSON(<the features>)`包装它来创建一个我们可以添加到地图的层。`raster-streamlines`也是如此。*

*该库还在每个特性的`properties`中提供 isoband 的数据范围。我们可以用它来设计等值线的样式或者给等值线添加标签。这是这个过程的另一个棘手的部分，我将很快介绍它。*

*您还需要考虑希望 web 地图使用哪种渲染器。默认情况下，传单使用 SVG。如果你有巨大的，详细的等带，性能将是糟糕的。相反，我选择了使用基于画布的渲染器，这带来了一些麻烦，但是一旦特性被绘制到画布上，就可以获得可接受的性能。*

*我们现在准备好样式我们的层。*

## *造型等带*

*这些是最容易造型的。您只需要传递一个样式函数:*

```
*L.geoJSON(<the features>, { style: feature => {
    return {
        fillColor: getColor( feature.properties[ 0 ].lowerValue ),
        weight: 1.5,
        opacity: 1,
        color: getColor( feature.properties[ 0 ].lowerValue ),
        fillOpacity: 1
    };
});*
```

*`getColor()`是我写的一个函数，它获取一个值，并从前面提到的适当色标中获得插值颜色(使用`tinygradient`)。由于`raster-marching-squares`，已经用数值范围填充了`feature.properties`。*

*你会注意到我还设置了 1.5 的描边/轮廓权重，因为如果多边形没有轮廓的话，它们之间通常会有很小的缝隙。有了轮廓，一个重要的警告是，如果你降低图层的不透明度(通过样式功能)，填充和轮廓的重叠将变得明显。相反，您需要将图层放在它自己的地图窗格上，并更改整个窗格的不透明度。*

*![](img/f03bbde0029a5698462e12606af9aaa0.png)*

*这篇文章不包括适当的制图色标，尤其是色盲。*

## *设计等值线*

*设计等值线本身非常简单:*

```
*return feature => {
    return {
        weight: 1.5,
        color: '#000000'
    }
}*
```

*但是标签呢？这其实很棘手。因为我们使用的是画布渲染器，所以绘制折线标签的传单插件是不可能的，因为它只适用于 SVG。我也不想对每一层使用不同的渲染器，比如在等值线上叠加等高线。*

*在这种情况下，我发现了一个名称尴尬的插件([fleet . street labels](https://github.com/triedeti/Leaflet.streetlabels))，它通过实际扩展渲染器，在画布渲染器中沿着折线绘制标签(因此它不是一个逐层解决方案)。它有几个依赖项(其中一个不能通过 npm 获得),所以设置它并不简单。*

*要使标签正常工作，有许多必要的修改。*

1.  *默认情况下，插件不能解析`feature.properties`,因为等值线函数将它放在一个数组中。它在寻找一个键，所以它期望`feature.properties`是一个对象。*
2.  *这个插件有一个过滤器，这样它就不会试图在所有的特征上画标签，只是那些有适当属性对象的特征。出于某种原因，我想可能是由于 GeoJSON 文件可以在野外找到，它通过检查属性是否被设置为字符串值`'undefined'`来检查属性是否未定义。因此，如果该键根本不存在，它仍然会尝试在该特征上绘制，这会产生一个错误。我需要修改插件来做到这一点(你可以在这个过程中处理第一个问题)。*
3.  *绘图问题。最大的问题是它喜欢倒着画标签(即使是在其 GitHub 页面的截图中)，这是糟糕的制图。一个较小的问题是，标签遵循轮廓的曲线，这很好，除了在标签变得不可读的急转弯处。*

*最后，我将代码和先决条件下载到我的代码库中，这样我就可以修改插件来更有效地过滤和读取特性属性。为了解决标签颠倒的问题，我修改了基本依赖关系( [Canvas-TextPath](https://github.com/Viglino/Canvas-TextPath) )，如果所有字母颠倒，就将它们翻转 180 度，同时颠倒它们的顺序。我还在轮廓旋转的最大值上加了一个大夹子，这样就不会在曲线上产生不可读的标签。有效的等高线标注是有争议的——完全水平的标注将等高线一分为二也不一定有什么错。*

*也有一些关于设置自定义字体的“todo”评论，所以我将 Roboto 放入画布绘制命令中，以匹配我网站布局的其余部分。也许有一天我会提出一个拉取请求:)*

*对于上面列出的第 1 点，我们可以通过在 isobands GeoJSON 层的初始化中做一些工作来规避数组的问题:*

```
*L.geoJSON( ..., {
    style,
    onEachFeature: ( feature, layer ) => {
        if ( !feature.properties[ 0 ] ) return;
        const contour = feature.properties[ 0 ].value;
        feature.properties = {
            contour // probably want to convert unit, add suffix :)
        }
    }
}*
```

*最后，在渲染器的初始化中，我让它过滤掉路径过小的形状(压扁的标签)，打开标签碰撞，并设置填充/描边颜色。这就是 TypeScript 令人讨厌的地方，因为这些粗略的插件没有定义(尽管我很欣赏它们的存在)。*

```
*const options: any = {
    collisionFlg: true,
    propertyName: 'contour',
    fontStyle: {
        ...
    }
}return new ( L as any ).ContourLabels( options );*
```

*这一切都很烦人，而且非常糟糕，但是渲染器实际上工作得很好——当你平移和缩放时，它会重新绘制标签。*

*![](img/8693b24dff5ca1062fa623b174b70623.png)*

*真的有用！(覆盖在相同数据的等带之上)。*

## *造型流线*

*流线稍微容易一点，因为有一个传单插件，允许沿着折线放置任意形状或标记(但不是文本，这就是为什么我们不得不采用上述等高线策略)。*

*插件是[传单。PolylineDecorator](https://github.com/bbecquet/Leaflet.PolylineDecorator) 它做得很好。一旦生成了 streamline 层，就可以解析它以获得插件期望的`LineString`特性:*

```
*const lines = layer.getLayers()
    .filter( ( layer: any ) => { 
        return layer.feature.geometry.type === "LineString"; 
    } )
    .map( (layer: any ) => {
        const coordinates = layer.feature.geometry.coordinates;
        coordinates.forEach( coordinate => { 
            coordinate.reverse(); 
        } );
        return L.polyline( coordinates );
    } );*
```

*是的，你可以看到我的代码库中有一些打字问题。*

*我们还需要为装饰者设置模式，这可能是沿着这条线的某种箭头。我猜插件会自动处理装饰器的旋转。很好。*

```
*const pattern = {
    offset: '10%',
    repeat: '20%',
    symbol: L.Symbol.arrowHead( {
        pixelSize: 8,
        polygon: true,
        pathOptions: {
            stroke: false,
            fill: true,
            fillColor: '#000000',
            fillOpacity: opacity
        }
     } )
}*
```

*现在把它传递给插件:*

```
*const streamlineArrows = L.polylineDecorator( lines, {
    patterns: [ pattern ]
} );*
```

*最后，为了便于管理，您可能希望将装饰器和流线层本身(姑且称之为`layer`)合并成一个单一的`LayerGroup`。*

```
*L.layerGroup ([layer, streamlineArrows]);*
```

*搞定了。我遇到的唯一问题是，插件总是将装饰器放在叠加地图窗格上(即使我试图绕过它)，这意味着如果你有基于窗格的图层排序，你会遇到一些 z 索引问题。也许需要另一个拉取请求…*

*![](img/7113a793c082b9539d2deb579e8563cd.png)*

*10m 风流线覆盖在表面阵风等带上。*

# *5.添加其他功能*

*我们的地图现在工作得很好，只要你从上面的例子代码中推断出来。但是如果我们想要分发可视化效果呢？我们当然可以使用截图工具(或等效工具)手动截图，但这是不一致的。不幸的是，我们没有什么好的选项来下载我们看到的地图，因为并不是所有的东西都被渲染到画布上(工具提示、注释等等)。).*

*本着保持̶s̶k̶e̶t̶c̶h̶y̶的精神…我的意思是，在客户端，我们可以进一步增加我们的`node_modules`目录的密度，达到一个临界质量，并使用像 [dom-to-image](https://github.com/tsayen/dom-to-image) 这样的库，有希望将我们地图容器的整个 dom 树呈现为一个图像。这实际上开箱即用就能很好地工作，但是对于使用 CORS 的外部 web 字体和地图图层来说会有困难，所以要小心。*

*一旦你解决了这些问题，你就可以非常无缝地渲染地图了，但是你需要[文件保护程序](https://www.npmjs.com/package/file-saver)将图像保存到磁盘上。*

```
*import domtoimage from 'dom-to-image';
import { saveAs } from 'file-saver';const image = await domtoimage.toJpeg( document.getElementsByClassName( 'map-container' )[ 0 ], { bgcolor: '#ffffff' } )saveAs( image, `my_weather_model_map_works.jpg` );*
```

*`domtoimage`的性能不是很好，所以如果你希望自动创建动画 GIF，这似乎不是一个可行的方法，尽管也许有一种方法可以用 web workers 来实现。不幸的是，在使用 JavaScript 做任何事情时，我们肯定会遇到性能限制的阻碍。*

*你可以用这些数据做很多其他的事情，也许最著名的例子是那些花哨的[动画风地图](https://www.windy.com/?39.707,-105.029,5)。您可以查询它(提示:使用`geoTransform`)、生成统计数据、编写一个 Discord 服务器机器人…但是当然，您可以在服务器上更容易地完成这些工作。客户端渲染的亮点在于天气模型的可视化和交互性。除此之外，很难证明在 web 浏览器中做任何事情的开销和性能问题是合理的。*

# *鸣谢和来源*

*这当然不是处理事情的唯一方式。查看正在做的事情[传单。比如 CanvasLayer.Field](https://ihcantabria.github.io/Leaflet.CanvasLayer.Field/) 。这项技术的潜力正在迅速发展，我相信在未来几年内，将会有更健壮、更高效的工具问世。事实上，甚至还有 GDAL 的 [WebAssembly 包装器！](https://www.npmjs.com/package/gdal-js)*

*我要感谢 [Roger Veciana i Rovira](https://medium.com/u/26d510f9aca?source=post_page-----5ff2db881cb8--------------------------------) 提供了[精彩的资源](https://bl.ocks.org/rveciana)和[库](https://github.com/rveciana?tab=repositories)，让我看到了这一切的可能性，让我不用编写低级的光栅处理算法就可以立即开始构建。*

*如果你试图挖掘我的网站，它不是生产就绪，甚至没有客户端渲染可用。然而，所有代码和示例图像都来自一个工作测试版本。有一天…*