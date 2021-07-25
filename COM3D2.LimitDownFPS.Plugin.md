# COM3D2.LimitDownFPS.Plugin
ゲームが非アクティブ（別のウィンドウが上に来ている時など）の時にFPSに制限をかけるプラグイン  
デフォルトではアクティブ時の半分のFPSに制限します  
~~設定ファイルを用意するのが面倒だったので~~ 非アクティブ時のFPSを変える場合はソースを直接書き換えてください

## ダウンロード
[こちら](https://github.com/AshleyScarlet/Warehouse/releases/download/LimitDownFPS.1.0.0/COM3D2.LimitDownFPS.Plugin.dll)からどうぞ  
バージョンは何でも動くと思います（カスメでも動きそう）  
FPS制限を解除するプラグイン等とは相性が悪いかもしれません  

## ソース
```cs
namespace COM3D2.LimitDownFPS.Plugin
{
    [UnityInjector.Attributes.PluginName("LimitDownFPS")]
    [UnityInjector.Attributes.PluginVersion("1.0.0")]
    public sealed class LimitDownFPS : UnityInjector.PluginBase
    {
        private void OnApplicationFocus(bool hasFocus)
        {
            int value;
            if (hasFocus)
            {
                value = prevTargetFrameRate == -1 ? UnityEngine.Application.targetFrameRate : prevTargetFrameRate;
            }
            else
            {
                prevTargetFrameRate = UnityEngine.Application.targetFrameRate;

                // 非アクティブ時のFPS
                // デフォルト :  prevTargetFrameRate / 2 (アクティブ時の半分)
                value = prevTargetFrameRate / 2;
            }
            UnityEngine.Application.targetFrameRate = value;
        }

        private static int prevTargetFrameRate = -1;
    }
}

```
🤔
