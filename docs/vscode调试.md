{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "program": "${workspaceFolder}/src/index.ts"
        },

        {
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome against localhost",
            "url": "http://dev.jd.com:5173/",
            "webRoot": "${workspaceFolder}",
            "userDataDir": false,
            "runtimeArgs": [
                "--disable-web-security",
                "--disable-features=VizDisplayCompositor"
            ]
        }
    ]
}



{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "program": "${workspaceFolder}/src/index.ts"
        },

        {
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome against localhost",
            "url": "http://dev.jd.com:5173/",
            "webRoot": "${workspaceFolder}",
            // 使用指定的用户数据目录 - "userDataDir": "${workspaceFolder}/.vscode/chrome-debug-profile" 会创建一个专门的 Chrome 配置文件目录
            "userDataDir": "${workspaceFolder}/.vscode/chrome-debug-profile"
        }
    ]
}



// attach 模式
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "attach",
            "name": "Attach to Chrome",
            "port": 9222,
            "webRoot": "${workspaceFolder}/src"
        }
    ]
}
Start-Process "chrome.exe" -ArgumentList "--remote-debugging-port=9222","--user-data-dir=$env:LOCALAPPDATA\Google\Chrome\User Data","http://dev.jd.com:5173/"
