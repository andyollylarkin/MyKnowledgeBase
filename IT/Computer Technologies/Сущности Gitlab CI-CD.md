---
aliases: [gitlab ci, gitlab ci/cd, gitlab pipeline, gitlab jobs]
tags: [cicd, gitlab, devops]
---
<h6>02-12-2022</h6>
----------

## Из чего состоит CI/CD в GitLab
- **Pipelines** -верхнеуровневый  компонент который включает все остальные. По сути все части описанные в *.gitlab-ci.yml* файле являются пайплайном.
![[Pasted image 20221202020756.png]]
- **Stages** - набор **Jobs**. Описывается в секции *stages*.
![[Pasted image 20221202021126.png]]
![[Pasted image 20221202021225.png]]
- **Jobs** - группа заданий в одной стадии (stages).
![[Pasted image 20221202021245.png]]


---
## Библиография
- [Gitlab Docs](https://docs.gitlab.com/ee/ci/pipelines/)
