# 1. ROS2 환경 세팅하기

**Reference :** https://docs.ros.org/en/kilted/Installation/Ubuntu-Install-Debs.html

이 글은 Ubuntu 24.04 LTS 버전을 기준으로 방법을 설명합니다.

Ubuntu가 설치되어있지 않다면 아래 페이지를 참고하세요.
* **[Ubuntu 설치하기/](../../LInux/)**

## 1. locale 설정하기

UTF-8을 지원하는 locale이 설정되어있어야 한다.

공식문서에 따르면, 영어가 아니더라도 UTF-8을 지원하는 locale이면 상관없다.

하지만 지금은 공식문서를 그대로 따라가는 방식으로 en_US.UTF-8을 적용했다.

```bash
locale  # check for UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verify settings
```

## 2. 필요 Repository 설치

### 1) universe Repository 허용

한국은 기본적으로 KAIST Repository를 사용한다.

ROS는 universe Repository를 사용하기 때문에, universe Repository를 허용한다.

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```

### 2) GPC 키 등록 및 Source List 추가

```bash
sudo apt update && sudo apt install curl -y
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F\" '{print $4}')
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb
```

(1) `curl`(서버에서 데이터를 다운로드하는 도구)을 install한다.

(2) Github API를 호출하여, ros-infrastructure/ros-apt-source 저장소의 최신 릴리스 정보를 JSON 형식으로 가져와, `‘tag-name’`에 해당하는 value값을 `ROS_APT_SOURCE_VERSION`에 저장한다.

(3) ROS 2 패키지 저장소 설정을 위한 설치 파일(.deb)를 다운로드한다.

(4) `curl` 명령어로 다운로드한 `.deb` 파일을 실제로 시스템에 설치한다. 이 명령어를 통해서 ROS 2 패키지를 다운로드 할 수 있는 **저장소를 설정**한다.

- `dpkg`  : Ubuntu에서 패키지를 관리하는 tool

### 3) 개발자 도구 설치

ROS와 관련된 툴을 추가하기 위해 개발자 도구를 먼저 설치한다.

```bash
sudo apt update && sudo apt install ros-dev-tools
```

## 3. ROS 2 설치하기

```bash
sudo apt update
sudo apt upgrade
sudo apt install ros-kilted-desktop
```

## 4. 환경 설정하기

시스템에 ros2라는 명령어를 알아듣게 하기 위해 터미널 창에 ROS 2 관련 명령어와 경로 정보를 등록한다.

```bash
source /opt/ros/kilted/setup.bash
```

- 현재 열려있는 터미널에서만 경로 정보가 등록되었기 때문에, 새로운 터미널을 열 때마다 이 명령어를 실행해야 한다.

## 5. Try Some Examples

두개의 터미널을 실행 후, 각각의 터미널에 다음과 같은 명령어를 입력한다.

- 첫 번째 터미널 (`C++ talker`)
    
    ```bash
    source /opt/ros/kilted/setup.bash
    ros2 run demo_nodes_cpp talker
    ```
    
- 두 번째 터미널 (`Python listener`)
    
    ```bash
    source /opt/ros/kilted/setup.bash
    ros2 run demo_nodes_py listner
    ```
    

![잘 작동되고 있음을 확인할 수 있다.]
(./etc/01_setup_environment_01.png)

잘 작동되고 있음을 확인할 수 있다.

---

> **© 2025 Urocontrol**, published on 24.12.2025