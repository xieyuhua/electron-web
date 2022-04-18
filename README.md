# electron-web

function createWindow () {
  // Create the browser window.
  const mainWindow = new BrowserWindow({
    width: 1024,
    height: 650,
    minHeight: 650,
    minWidth: 820,
    autoHideMenuBar: true,
    frame: false,
    webPreferences: {
      nodeIntegration: true,
      contextIsolation: false,
      preload: path.join(__dirname, 'preload.js')
    }
  })

  // and load the index.html of the app.
  mainWindow.loadFile('index.html')

    // Open the DevTools.
   //mainWindow.webContents.openDevTools()

    const ipc = ipcMain
    //登录窗口最小化
    ipc.on('min',function(){
        mainWindow.minimize()
    })
    //登录窗口最大化
    ipc.on('max',function(){
        if(mainWindow.isMaximized()){
            mainWindow.restore()
        }else{
            mainWindow.maximize()
        }
    })
    ipc.on('close',function(){
        mainWindow.close()
        app.quit()
    })

    ipc.on('show',function(){
        mainWindow.show()
    })

}

// This method will be called when Electron has finished
// initialization and is ready to create browser windows.
// Some APIs can only be used after this event occurs.
app.whenReady().then(() => {
  createWindow()
  
  app.on('activate', function () {
    // On macOS it's common to re-create a window in the app when the
    // dock icon is clicked and there are no other windows open.
    if (BrowserWindow.getAllWindows().length === 0) createWindow()
  })
})

// Quit when all windows are closed, except on macOS. There, it's common
// for applications and their menu bar to stay active until the user quits
// explicitly with Cmd + Q.
app.on('window-all-closed', function () {
  if (process.platform !== 'darwin') app.quit()
})
