
## 01. 서버 사용자 추가

1. 사용자를 추가한다. (adduser 명령어의 경우 홈 디렉토리도 함께 생성한다.)

```bash
sudo adduser [USERNAME]
```

2. 사용자의 권한을 할당한다.

```bash
sudo vi /etc/group

## in /etc/group
sudo:x:user1,user2, ... # <- 사용자 아이디 추가
```


## 02. SAMBA 설정

다른 컴퓨터에서 파일 탐색기를 통해 해당 서버의 디렉토리에 접근하고자 할 때 필요한 설정이다.

1. samba 사용자를 추가한다.

```bash
sudo smbpasswd -a [USERNAME]
```

2. samba 설정 파일에 유저 정보를 추가한다.
    1. `[NAME]`: samba 접속 시 사용할 경로, path의 별칭
    2. `path`: 해당 이름으로 접속했을 때 연결할 디렉토리
    3. `valid users`: 해당 경로에 접근할 수 있는 사용자 아이디

```python
## in /etc/samba/smb.conf

#======================= Share Definitions =======================
...

[sujin_home]
   path = /home/sujin
   valid users = sujin
   read only = no
   writable = yes
   public = no
   browseable = yes
   printable = no
   create mask = 0750

[sujin_data1]
   path = /mnt/data1/sujin
   valid users = sujin
   read only = no
   writable = yes
   public = no
   browseable = yes
   printable = no
   create mask = 0750

[specific_dir_shared]
   path = /mnt/data1/sujin/project_A
   read only = no
   writable = yes
   public = yes
   browseable = yes
   printable = no
   create mask = 0750
   
...
```


### 1) samba 접속

#### MacOS

1. Finder에서 `이동 > 서버에 연결` 또는 `command + k`로 네트워크 서버 연결 창에 접속한다.
2. `smb://[SERVER_IP or DNS]/[NAME]/[DIR_PATH]`와 같은 형식을 입력한다.
	1. (예) `smb://10.100.00.000/sujin_home/project_B/documents`  
	   → `10.100.00.000` 서버의 `sujin_home(=/home/sujin)` 디렉토리 아래 `project_B/documents` 디렉토리를 연결  


## 03. 방문자 계정으로 접근하기

접근하고자 하는 서버의 디렉토리에 특정 사용자가 아닌 방문자 권한으로 접근하고자 할 때,  
1. samba 설정 파일 `/etc/samba/smb.conf`의 `hosts allow`에 본인 기기의 ip를 추가해야 한다.


#### 🅐 연구실 공유 디렉토리 접근을 위한 설정 예시

<h2>API 호출 및 결과 출력 예제</h2>

<label for="inputString">문자열 입력:</label>
<input type="text" id="inputString">
<button onclick="callApi()">제출</button>

<div id="resultContainer"></div>

<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script>
function callApi() {
    // 입력된 문자열 가져오기
    var inputString = document.getElementById('inputString').value;

    // API 호출 (GET 메서드 사용)
    $.ajax({
        url: 'https://api.example.com/your-api-endpoint?input=' + encodeURIComponent(inputString),
        type: 'GET',
        success: function(response) {
            // API 호출이 성공하면 결과를 출력
            displayResult(response);
        },
        error: function(error) {
            // API 호출이 실패하면 오류 메시지 출력
            displayResult('API 호출 오류: ' + error.statusText);
        }
    });
}

function displayResult(result) {
    // 결과를 출력할 요소 가져오기
    var resultContainer = document.getElementById('resultContainer');

    // 결과를 요소에 추가
    resultContainer.innerHTML = '<p><strong>API 호출 결과:</strong> ' + result + '</p>';
}
</script>










