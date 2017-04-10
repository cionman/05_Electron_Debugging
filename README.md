<img src="ASSETS/electron.png" alt="JavaScript">

# Electron Debugging.

Electron 공식 홈페이지에서는 Debugging과 관련하여 3가지 문서를 제공합니다.

- [Debugging the Main Process](https://electron.atom.io/docs/tutorial/debugging-main-process/#debugging-the-main-process)
- [Debugging the Main Process in node-inspector](https://electron.atom.io/docs/tutorial/debugging-main-process-node-inspector/#debugging-the-main-process-in-node-inspector)
- [Debugging the Main Process in VSCode](https://electron.atom.io/docs/tutorial/debugging-main-process-vscode/)

node-inspector의 설정을 잘못한 탓인지는 몰라도..node-inspector의 디버깅은 원할할게 디버깅이 되지 않았습니다.

그에 반해 Visual Studio Code 에서는 깔끔하게 Main Process와 Renderer Process의 코드를 모두 디버깅할수 있었습니다. 그래서 Visual Studio Code의 디버깅 방법을 소개 합니다.

### Main Process Debugging 설정

1. Visual Studio Code로 프로젝트를 오픈합니다. 
2. 프로그램의 왼쪽 사이드바의 디버깅 메뉴로 들어갑니다.
3. 상단에 톱니바퀴를 클릭하고 Node.js를 선택합니다. 그러면 프로젝트에 **.vscode**폴더가 생성이 되고 하위에 **launch.json**파일이 생성됩니다.
<img src="ASSETS/3.png" alt="JavaScript">

4. launch.json 파일 화면 에 '구성추가' 버튼을 클릭하면 미리 구성되어 있는데 정보를 불러올 수 있는데 이 중에 'Node.js:Electron 주'를 선택하면 손쉽게 설정이 완료됩니다.
<img src="ASSETS/6.png" alt="JavaScript">
5. 만약 이런 부분을 찾을 수 없다면 launch.json파일에 아래와 같이 작성하면 됩니다.
```
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Electron Main",
            "runtimeExecutable": "${workspaceRoot}/node_modules/.bin/electron",
            "windows": {
                "runtimeExecutable": "${workspaceRoot}/node_modules/.bin/electron.cmd"
            },
            "program": "${workspaceRoot}/index.js",
            "protocol": "legacy"
        }
    ]
}
```
### Renderer Process Debugging 설정
Electron내의 webContents.openDevTools() 로 개발자도구를 오픈하여 렌더러 프로세스는 디버깅 할 수 있습니다.

Visual Studio Code로 Renderer Process Debugging을 하려면 Visual Studio Code의 확장 프로그램 중 '**Debugger for Chrome**'을 반드시 설치 해야합니다.

설치 후 아래의 코드를 launch.json 파일내 configurations 항목 의 배열내에 값을 추가합니다.

Main process  설정과 다른점은 type의 값이 Chrome이고, runtimeArgs항목이 추가되고 name 항목이 변경되었습니다.

주의할점은 Electron의 개발자도구를 켜면 Visual Studio Code에서는 디버깅이 되지 않습니다.
```
    {
            "type": "chrome",
            "request": "launch",
            "name": "Electron Renderer",
            "runtimeExecutable": "${workspaceRoot}/node_modules/.bin/electron",
            "windows": {
                "runtimeExecutable": "${workspaceRoot}/node_modules/.bin/electron.cmd"
            },
            "runtimeArgs": [
                "${workspaceRoot}",
                "--enable-logging",
                "--remote-debugging-port=9222"
            ],
            "program": "${workspaceRoot}/index.js",
            "protocol": "legacy"
        }
```

### 디버깅 실행
1. 코드에 원하는 곳에 Break Point를 지정합니다.
2. 디버깅 구성에서 원하는 설정을 선택한 후 실행버튼을 누릅니다.
<img src="ASSETS/7.png" alt="JavaScript">

## [Electron 유용한 링크 ➤](https://github.com/cionman/06_Electron_Useful_Link) 


 



