# organization-convention

# GPG GitHub 설정 가이드

이 레포지토리는 GPG(GNU Privacy Guard)를 설치하고 설정하여 GitHub 커밋에 서명하는 방법을 안내합니다.

## 목차
- [소개](#소개)
- [GPG 설치 방법](#GPG-설치-방법)
  - [Linux](#linux)
  - [macOS](#macos)
  - [Windows](#windows)
- [GPG 키 생성](#gpg-키-생성)
- [GPG 키 GitHub 계정에 추가](#gpg-키-github-계정에-추가)
- [Git 설정](#git-설정)
- [커밋 서명하기](#커밋-서명하기)
- [문제 해결](#문제-해결)
- [추가 자료](#추가-자료)

## 소개

GPG는 데이터 암호화 및 서명을 위한 도구입니다. GitHub에서 GPG를 사용하면 커밋에 서명하여 해당 커밋이 신뢰할 수 있는 소스에서 왔음을 검증할 수 있습니다. 이 가이드는 GPG를 설치하고 설정하여 GitHub 커밋에 서명하는 방법을 단계별로 안내합니다.

## GPG 설치 방법

### Linux

Debian/Ubuntu:
```bash
sudo apt update
sudo apt install gnupg
```

Fedora:
```bash
sudo dnf install gnupg
```

Arch Linux:
```bash
sudo pacman -S gnupg
```

### macOS

Homebrew를 사용한 설치:
```bash
brew install gnupg
```

### Windows

1. [Gpg4win](https://www.gpg4win.org/) 웹사이트에서 설치 프로그램을 다운로드합니다.
2. 다운로드한 설치 프로그램을 실행하고 안내에 따라 설치합니다.

## GPG 키 생성

1. 터미널/명령 프롬프트를 열고 다음 명령어를 실행합니다:

```bash
gpg --full-generate-key
```

2. 키 종류를 선택합니다. GitHub는 RSA와 ElGamal을 모두 지원합니다.
   - 기본 값인 RSA를 선택합니다.

3. 키 크기를 지정합니다. GitHub는 최소 4096비트를 권장합니다.
   - `4096`을 입력하세요.

4. 키 만료 기간을 설정합니다.
   - 만료되지 않는 키를 원한다면 `0`을 입력하세요.
   - 보안을 위해 2년 만료 기간을 설정하는 것이 좋습니다.

5. 이름, 이메일 주소 및 선택적으로 코멘트를 입력합니다.
   - 이메일 주소는 GitHub 계정에 등록된 이메일과 일치해야 합니다.

6. 본인이 사용할 암호를 입력하세요.

<img width="608" alt="image" src="https://github.com/user-attachments/assets/0a451039-da4e-4c6a-afa2-c506095d0910" />


<br>

## GPG 키 GitHub 계정에 추가

1. GPG 키 ID를 확인합니다:

```bash
gpg --list-secret-keys --keyid-format=long
```

<img width="578" alt="image" src="https://github.com/user-attachments/assets/d30f6296-4769-45e9-adb0-6b8446163bcb" />

<br><br>

2. GPG 공개 키를 내보냅니다:

```bash
gpg --armor --export <KEY ID>
```

3. 전체 GPG 키 출력(BEGIN PGP PUBLIC KEY BLOCK부터 END PGP PUBLIC KEY BLOCK까지)을 복사합니다.

<img width="629" alt="image" src="https://github.com/user-attachments/assets/b5dcdb16-dd75-4d55-b170-4bc5875a2312" />

<br><br>


4. GitHub 계정에 GPG 키를 추가합니다:
   - GitHub에 로그인합니다.
   - 오른쪽 상단의 프로필 아이콘을 클릭한 다음 `Settings`를 선택합니다.
   - 왼쪽 사이드바에서 `SSH and GPG keys`를 클릭합니다.
   - `New GPG key` 버튼을 클릭합니다.
   - 복사한 GPG 키를 입력 필드에 붙여넣고 `Add GPG key`를 클릭합니다.
   - 필요한 경우 GitHub 비밀번호를 확인합니다.

<br>

## Git 설정

1. Git에서 GPG 키를 사용하도록 설정합니다:

```bash
git config --global user.signingkey <KEY ID>
```

2. 커밋 서명을 활성화합니다:

```bash
git config --global commit.gpgsign true
```

3. (선택) macOS 또는 Linux에서 GPG 비밀번호를 캐시하려면:

```bash
echo "pinentry-program /usr/local/bin/pinentry-mac" >> ~/.gnupg/gpg-agent.conf
```

macOS의 경우 pinentry-mac을 설치해야 할 수 있습니다:
```bash
brew install pinentry-mac
```

4. (선택) Windows에서 GPG 비밀번호를 캐시하려면:
   - Gpg4win이 설치한 Kleopatra 애플리케이션을 사용합니다.
   - 설정 > 비밀번호 캐시 기간을 조정합니다.

## 커밋 서명하기

1. 자동으로 모든 커밋에 서명하도록 Git을 설정했다면 평소처럼 커밋하면 됩니다:

```bash
git commit -m "Your commit message"
```

2. 개별 커밋에만 서명하려면:

```bash
git commit -S -m "Your commit message"
```

3. 서명이 올바르게 작동하는지 확인하려면:

```bash
git log --show-signature
```

## 추가 자료

- [GitHub의 커밋 서명 문서](https://docs.github.com/en/authentication/managing-commit-signature-verification)
- [GnuPG 공식 문서](https://gnupg.org/documentation/)
- [Git 도구 - 작업에 서명하기](https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work)
