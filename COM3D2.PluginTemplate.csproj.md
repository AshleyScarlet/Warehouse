VisualStudioでオダメ向けのプラグインをビルドできるようにするcsprojファイル  
ほぼ自分向けですがご自由にどうぞ  

```csproj
<Project Sdk="Microsoft.NET.Sdk" InitialTargets="BuildCOM3D2Plugin" DefaultTargets="BuildCOM3D2Plugin">

	<PropertyGroup>
		<OutputType>Library</OutputType>
		<TargetFramework>net45</TargetFramework>
		<AllowUnsafeBlocks>true</AllowUnsafeBlocks>
		<LangVersion>Latest</LangVersion>
		<GenerateTargetFrameworkAttribute>false</GenerateTargetFrameworkAttribute>

		<!-- オダメがインストールされているパス -->
		<!-- レジストリから取得するようにしてます -->
		<COM3D2Path>$(registry:HKEY_CURRENT_USER\SOFTWARE\KISS\カスタムオーダーメイド3D2@InstallPath)</COM3D2Path>
		<COM3D2Managed>$(COM3D2Path)COM3D2x64_Data\Managed\</COM3D2Managed>

		<!-- Monoのコンパイラー(MCS)のパスを指定します -->
		<!-- 人によってインストール場所が違うはずなので確実に指定してください -->
		<MCSPath>"C:\Program Files\Mono\bin\mcs"</MCSPath>

		<!-- Sybarisのプラグイン（～.Patcher.dll）としてビルドする場合はTrue、 -->
		<!-- UnityInjectorのプラグイン（～.Plugin.dll）としてビルドする場合はFalse -->
		<IsSybarisPlugin>False</IsSybarisPlugin>

	</PropertyGroup>

	<ItemGroup>
		<!-- 参照を追加する場合はこちらに手動で追記してください -->
		<Library Include="$(COM3D2Managed)Assembly-CSharp.dll"></Library>
		<Library Include="$(COM3D2Managed)UnityEngine.dll"></Library>

		<!-- <Library Include="$(COM3D2Managed)UnityEngine.UI.dll"></Library> -->

		<Library Condition="!$(IsSybarisPlugin)" Include="$(COM3D2Path)Sybaris\UnityInjector.dll"></Library>
		<Library Condition="$(IsSybarisPlugin)" Include="$(COM3D2Path)Sybaris\Mono.Cecil.dll"></Library>
	</ItemGroup>

	<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->

	<ItemGroup>
		<Reference Include="@(Library)">
			<HintPath>@(Library)</HintPath>
			<Private>false</Private>
		</Reference>
	</ItemGroup>

	<Target Name="BuildCOM3D2Plugin" AfterTargets="Build">
		<PropertyGroup>
			<References>-r:@(Library, ' -r:')</References>
		</PropertyGroup>
		<Message Text="Build COM3D2 Plugin." Importance="high" Condition="true" />
		<MakeDir Directories="$(OutDir)" />
		<Delete Files="$(OutputPath)$(AssemblyName).dll" />
		<Exec Command="$(MCSPath) -langversion:Experimental -unsafe -optimize -t:library -out:$(OutputPath)$(AssemblyName).dll $(References) @(Compile, ' ')" />
	</Target>

</Project>
```
