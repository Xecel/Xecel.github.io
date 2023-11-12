---
title: Ec2(fedora, arm) docker 설치
date: 2023-11-03 00:00:00 +0900
lastmod: 2023-11-03 00:00:00 +0900
categories: [AWS, EC2]
tags: [AWS, EC2]
---

<style>
.env-text {
  color: #0f52ba;
}
.command-text {
  color: #8B00FF;
}
</style>

<style>
blockquote {
  display: block;
  background-color: #f8f9fa;
  color: #000080;
}
</style>

---

## 문제

> ![1_ec2_env](/assets/img/2023-11-03/1_ec2_env.jpg)
>
> Ec2(fedora, arm) 생성 후 docker 설치 시 알고 있던 명령어로 설치가 안 된다.
>
> ![2_docker_installation_failed](/assets/img/2023-11-03/2_docker_installation_failed.jpg)

## 해결

> ![3_cmd](/assets/img/2023-11-03/3_cmd.jpg)
>
> **_<code class="command-text">sudo yum update -y</code>_**

> ![4_intalling](/assets/img/2023-11-03/4_intalling.jpg)
>
> **_<code class="command-text">sudo yum install docker -y</code>_**

> ![5_installed](/assets/img/2023-11-03/5_installed.jpg)

## 원인

> **apt - Debian 기반**  
> **yum - Red Hat 기반**
>
> linux 기반별로 사용하는 패키지 관리자가 달라 발생한 문제, 많이 사용하는 Ubuntu linux 에서는 apt를 사용하지만 Fedora는 Red Hat 기반이라 yum을 사용해야 한다.
