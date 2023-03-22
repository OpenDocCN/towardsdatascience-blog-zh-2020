# 设置版本增量和自动发布流程

> 原文：<https://towardsdatascience.com/setup-version-increment-and-automated-release-process-591d87ea1221?source=collection_archive---------5----------------------->

## 我将向您展示发布过程和版本增量的自动化有多容易

![](img/f854f5447e972f340f8de0db9f56c675.png)

照片由[扬西·敏](https://unsplash.com/@yancymin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我热衷于过程自动化，并且我坚信在开发阶段对过程自动化的时间和金钱投资是首要需求，无论项目持续多长时间。当您投资于流程自动化时，您将进一步大幅降低维护费用。

我认为 ML 文章中没有适当涉及的一个主题是发布过程自动化和项目版本增量。我不想在这个帖子中讨论可能的发布选项，这不是关于它的。在这里，我们将讨论如何设置 CI/CD 工具的发布流程(Bitbucket 管道、Gitlab 管道等)。)我认为可以肯定地说，在大多数情况下，release 是一个文件或一组文件，它们将以某种方式集成到客户端现有的解决方案中(无论它是一个包含 spring boot 应用程序的 jar 文件，一组用于模型训练和预测的 python 脚本，还是一个运行核心应用程序的 docker 映像)。问题陈述:我们希望管理产品版本，我们希望能够返回到具体的版本，以便能够测试和诊断生产中发生的问题。

## 版本控制

首先，让我们定义版本字符串中的数字是什么意思。

![](img/745431cacdef0ad7dfe3c42c90fa77ef.png)

版本字符串

从语义版本定义(【https://semver.org/】T4)

给定一个版本号**少校。MINOR . PATCH-pre _ release-label**，递增:

1.  **主要**版本当您做出不兼容的 API 更改时
2.  以向后兼容的方式添加功能时的次要版本
3.  **补丁**版本当你做向后兼容的 bug 修正。

**预发布标签**是版本的占位符，在发布前保留。

这是一个最佳实践定义，但是当然，您可以选择自己的版本化方案。所以根据这个定义，我们需要定义三个任务:

1.  发布并增加主要版本
2.  释放并增加次要版本
3.  发布并增加补丁版本

下一步是定义除了 git 标记之外，如何在 GIT 历史中表示发布。有几种技术可以实现这一点，最流行的是分离发布分支或分离提交。在本帖中，我们将讨论分离提交，因为它更容易自动化(不需要合并)。

## 分离提交

让我们从理论开始。分离提交不属于任何分支，只能直接指向。在不知道提交散列的情况下，如何在 git 历史中搜索它？你只需要给它分配一个 git 标签。然后，您可以在任何 VCS UI 中通过标签过滤 git 历史。

## 版本文件

不知何故，你必须保持实际的版本字符串。最简单的方法是将它保存在存储库根目录下的文本文件中。版本文件的策略:

1.  不能手动更改文件内容
2.  它不能在特征分支中改变(最佳实践是创建一个流水线步骤，它与主步骤进行比较)
3.  文件仅在发布后更改，并自动直接提交到主分支。

## 设置 CI/CD 管道

对于您的 CI/CD 管道文件，您必须添加三个定制/手动步骤/作业，它们将执行 release 和适当的增量。并选择性地添加步骤/作业，该步骤/作业仅在功能分支上运行，并检查版本文件是否根据主文件进行了更改。

Bash 脚本检查版本文件是否相对于主文件发生了更改:

```
git diff --exit-code VERSION
```

现在是最有趣的部分:

1.  我们必须创建一个脚本来管理版本文件的功能:获得版本，获得版本与占位符，增加版本主要/次要/补丁
2.  我们必须定义发布分支限制策略(发布只能在主分支上进行)并创建分离的提交推送流。
3.  定义 CI/CD 释放步骤(这里我们将为 Bitbucket 管道和 Gitlab 管道定义它)

我讨厌写 bash 脚本。我只使用 bash 来定义高级步骤并从中运行 python 脚本，因为我坚信 bash 脚本具有难看的符号，对于经验较少的 Linux 用户来说不可读，并且从长期角度来看不可维护。`fire`库可以帮助你轻松编写好的 CLI-runnable python 脚本。查看我之前的帖子:

[](/a-simple-way-to-create-python-cli-app-1a4492c164b6) [## 创建 python CLI 应用程序的简单方法

### 将机器学习应用程序包装到命令行界面的简单示例

towardsdatascience.com](/a-simple-way-to-create-python-cli-app-1a4492c164b6) 

版本文件管理脚本(没什么有趣的讨论，只是文件写操作):

```
#!/usr/bin/env python
import os
import re

import fire

pre_release_placeholder = 'SNAPSHOT'
version_filepath = os.path.join('.', 'VERSION')
version_pattern = re.compile(fr'^\d+.\d+.\d+(-{pre_release_placeholder})?$')

def get(with_pre_release_placeholder: bool = False):
    with open(version_filepath, 'r') as version_file:
        version_lines = version_file.readlines()
        assert len(version_lines) == 1, 'Version file is malformed'
        version = version_lines[0]
        assert version_pattern.match(version), 'Version string is malformed'
        if with_pre_release_placeholder:
            return version
        else:
            return version.replace(f'-{pre_release_placeholder}', '')

def write_version_file(major: int, minor: int, patch: int):
    version = f'{major}.{minor}.{patch}-{pre_release_placeholder}'
    with open(version_filepath, 'w') as version_file:
        version_file.write(version)

def inc_patch():
    version = get()
    major, minor, patch = version.split('.')
    write_version_file(major, minor, int(patch) + 1)

def inc_minor():
    version = get()
    major, minor, patch = version.split('.')
    write_version_file(major, int(minor) + 1, patch)

def inc_major():
    version = get()
    major, minor, patch = version.split('.')
    write_version_file(int(major) + 1, minor, patch)

if __name__ == "__main__":
    fire.Fire({
        'get': get,
        'inc-patch': inc_patch,
        'inc-minor': inc_minor,
        'inc-major': inc_major
    })
```

现在让我们开发发布脚本。首先，我们必须检查发布脚本是否运行在正确的分支上:

```
# Bitbucket
commit=${BITBUCKET_COMMIT:-$(git rev-parse HEAD)}# Gitlab
commit=${CI_COMMIT_SHA:-$(git rev-parse HEAD)}# Define ALLOWED_RELEASE_BRANCH in VCS pipelines secret variables.
branch=${ALLOWED_RELEASE_BRANCH:-master}

if ! git branch -a --contains "${commit}" | grep -e "^[* ]*remotes/origin/${branch}\$"
then
  echo -e "###\n### Not on ${branch}. Only ${branch} commits can be released.\n###"
  exit 1
else
  echo -e "###\n### Releasing of ${commit} on ${branch}\n###"
fi
```

然后你必须写一些释放的东西，如:发布 docker 图像或复制到 S3 水桶或任何你需要的。不要忘记使用正确的版本文件:

```
version=$(./scripts/version.py get)
version_file=VERSIONecho ${version} > ${version_file}
```

然后，我们必须在之前的步骤版本文件中使用 modified 推送一个分离的提交:

```
# Define some cool release pusher to ditinguish from amnual commits :)
git config user.name "Elon Musk"
git config user.email "elon.musk@spacex.com"

echo "Pushing detached tag of new version"
git add ${version_file}
git commit -m "Release version ${version}"
git tag  -a ${version} -m "Release version ${version} tag"
git push origin ${version}
```

取回主分支并使用预发布占位符增加版本:

```
echo "Pushing new version to ${branch}"
git fetch origin "${branch}:${branch}" || git pull
git checkout "${branch}"# Parameter of the script
release_type=${1}
./scripts/version.py inc-${release_type}

next_working_version=$(./scripts/version.py get --with-pre-release-placeholder)
git add ${version_file}
git commit -m "Incrementing working version to ${next_working_version} after ${version} release."
git push origin ${branch}
```

仅此而已。我们已经创建了一个干净的发布脚本。现在让我们开发 Bitbucket 和 Gitlab 管道步骤片段。

**比特桶**:

```
image:
  name: python:3.7.6-slim-buster

pipelines:
  default:
    - step:
        - name: Test version file not changed
        - script:
            - git diff --exit-code VERSION

  custom:
    release-inc-patch:
      - step:
          caches:
            - pip
          name: Release current version and increment patch version
          script:
            - apt-get update && apt-get install -y git
            - pip install fire==0.2.1
            - ./scripts/release.sh patch

    release-inc-minor:
      - step:
          caches:
            - pip
          name: Release current version and increment minor version
          script:
            - apt-get update && apt-get install -y git
            - pip install fire==0.2.1
            - ./scripts/release.sh minor
    release-inc-major:
      - step:
          caches:
            - pip
          name: Release current version and increment major version
          script:
            - apt-get update && apt-get install -y git
            - pip install fire==0.2.1
            - ./scripts/release.sh major
```

运行自定义管道步骤:[https://confluence . atlassian . com/bit bucket/run-pipelines-manually-861242583 . html](https://confluence.atlassian.com/bitbucket/run-pipelines-manually-861242583.html)

**Gitlab** :

```
test-version-changed:
  stage: test
  name: Test version file not changed
  script:
    - git diff --exit-code VERSION

release-inc-patch:
  stage: deploy
  image: python:3.7.6-slim-buster
  caches:
    - pip
  name: Release current version and increment patch version
  script:
    - apt-get update && apt-get install -y git
    - pip install fire==0.2.1
    - ./scripts/release.sh patch
  when: manual
  only:
    - master

release-inc-minor:
  stage: deploy
  image: python:3.7.6-slim-buster
  caches:
    - pip
  name: Release current version and increment minor version
  script:
    - apt-get update && apt-get install -y git
    - pip install fire==0.2.1
    - ./scripts/release.sh minor
  when: manual
  only:
    - master

release-inc-major:
  stage: deploy
  image: python:3.7.6-slim-buster
  caches:
    - pip
  name: Release current version and increment major version
  script:
    - apt-get update && apt-get install -y git
    - pip install fire==0.2.1
    - ./scripts/release.sh major
  when: manual
  only:
    - master
```

运行手动作业:[https://forum . git lab . com/t/git lab-ci-run-pipeline-manually/13797](https://forum.gitlab.com/t/gitlab-ci-run-pipeline-manually/13797)

就是这样！你能在我的 GitHub 上找到的所有资源:【https://github.com/mbalatsko/release-version-increment】

**我希望这篇文章将是有用的和信息丰富的。期待您的反馈！**