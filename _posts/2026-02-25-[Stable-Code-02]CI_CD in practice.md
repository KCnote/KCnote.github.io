---
layout: post
title: "02. CI & CD in practice"
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

### <b> Current state </b>.
![CICD-Before](/assets/img/develop/CICD-Before.png)

> After

### <b> Now i will <span style="color:cyan">add new option "check in common" between build site and Test site</span></b>.


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

      - name: Test site
      ...
```

### <b> New state and processing </b>
![CICD-After1](/assets/img/develop/CICD-After1.png)

### <b> Finish the new process </b>
![CICD-After2](/assets/img/develop/CICD-After2.png)

### <b> Change the structure CI pipeline </b>
![CICD-Change](/assets/img/develop/CICD-Change.png)


### 2. If the "check in common" step have issue from forgetting the title or wrong world

### <b> Intentionally Add typo within "title" </b>
![CICD-Wrong](/assets/img/develop/CICD-Wrong.png)

### <b> CI process fails on "check in common" process and Fail to deploy </b>
![CICD-Error](/assets/img/develop/CICD-Error.png)

### <b> After correct the name of title, it works </b>
![CICD-Pass](/assets/img/develop/CICD-Pass.png)