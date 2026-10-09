# Проблема с внеплановым закрытием

В случае, если приложение RemoteApp открывается не с первого раза и/или закрывается во время работы со следующей ошибкой:

```
[FATAL][com.freerdp.winpr.assert] - [winpr_int_assert]: !overlapping(pDstData, nXDst, nYDst, nDstStep, FreeRDPGetBytesPerPixel(DstFormat), pSrcData, nXSrc, nYSrc, nSrcStep, FreeRDPGetBytesPerPixel(SrcFormat), nWidth, nHeight)
[ERROR][com.freerdp.utils.signal] - [fatal_handler]: Caught signal 'Аварийный останов'
```

Отключите на терминальном сервере политику RemoteFX:

```
[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services]

"fEnableRemoteFXAdvancedRemoteApp"=dword:00000000
```
