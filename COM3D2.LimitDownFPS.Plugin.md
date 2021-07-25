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
        private static int prevTargetFrameRate = -1;
        private void OnApplicationFocus(bool hasFocus)
        {
            int value = prevTargetFrameRate;
            if (!hasFocus)
            {
                prevTargetFrameRate = UnityEngine.Application.targetFrameRate;

                if (prevTargetFrameRate == -1)
                    value = 30;　                     // 無制限だった場合はとりあえず30にしておく
                else
                    value = prevTargetFrameRate / 2;  // 非アクティブ時のFPSを決定する
            }
            UnityEngine.Application.targetFrameRate = value;
        }
    }
}
```
🤔
