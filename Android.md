## emulator

### CLI (Command Line Interface) 에서 실행하기

#### 1. 사용자 환경변수 path 추가

- C:\Users\User\AppData\Local\Android\Sdk\platform-tools
- C:\Users\User\AppData\Local\Android\Sdk\emulator
- C:\Users\User\AppData\Local\Android\Sdk\tools\bin

#### 2. 최신 SDK tools 와 시스템 이미지 설치

```bash
>sdkmanager "platform-tools" "platforms;android-28"
>sdkmanager "system-images;android-28;google_apis;x86"
>sdkmanager --licenses
```

#### 3. AVD 설치

```bash
> avdmanager create avd -n Android28 -k "system-images;android-28;google_apis;x86"
```

#### 4. HAXM or Hyper-V  설치

##### (1) HAXM (Intel Hardware Accelerated Execution Manager)

- https://software.intel.com/en-us/articles/intel-hardware-accelerated-execution-manager-intel-haxm
- 애뮬레이터 실행하기 위해 설치가 필요함

##### (2) Hyper-V 비활성화

- https://www.nextofwindows.com/how-to-enable-configure-and-use-hyper-v-on-windows-10
- Hyper-V 가 활성화되어 있다면 HAXM 을 실행하기 위해 비활성화 해야함(관리자 권한으로)
  - bios 설정에서 비활성화

#### 4. 애뮬레이터 실행 가능 devices 확인

```bash
emulator -list-avds
```

#### 5. 애뮬레이터 실행

```bash
emulator -avd avd_name [ {-option [value]} … ]
```

```bash
>emulator -avd Android28
```

#### 6. 실행중인 안드로이드 디바이스 확인

```bash
>adb devices
```



## Firebase

- https://rnfirebase.io/

### 1. Basic Starter 

