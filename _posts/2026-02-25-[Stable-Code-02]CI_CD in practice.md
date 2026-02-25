---
layout: post
tit=le: "02. CI & CD in practice"
date: 2026-02-25 00:00:00 +0900
author: kang
categories: [Stable, CI/CD]
tags: [Stable Code, CI/CD]
pin: false
math: true
mermaid: true
---

---
# <b>Continuous Integration(CI) & Continuous Deployment(CD)</b>
---
### <b>Prerequisites</b>
    1. Understand the step of CI/CD

---
## <b>In pratice</b>

### 1. Add new option whether the github.io post have a the name of title or not?

After build site step, adding new option. In this case, for simple example and practice, i will add simple option whether the github post have a title or not.


### 2. Compare Before and After?

> Before

The process is $ Build site \rightarrow Test site $.

So i wanna add new option "check in common"

![CICD-Before](/assets/img/develop/CICD-Before.png)


```yml
      - name: Build site
      ...

      # new option
      - name: Check in common
        run: |
          echo "Check in common setting..."

          for file in _posts/*.md; do
            echo "Checking $file"

            if ! grep -q "^title:" "$file"; then
              echo "Missing title in $file"
              exit 1
            fi
          done

          echo "Check in common passed"

      - name: Check in common
      ...
```

![CICD-After1](/assets/img/develop/CICD-After1.png)
![CICD-After2](/assets/img/develop/CICD-After2.png)

> Change the structure CI pipeline
![CICD-Change](/asset/img/develop/CICD-Change.png)


### 2. If the "check in common" step have issue from forgetting the title or wrong world

![CICD-Wrong](/asset/img/develop/CICD-Wrong.png)