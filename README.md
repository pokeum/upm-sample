# UPM (Unity’s Package Manager)

> [!TIP]
> - [Unity’s Package Manager](https://docs.unity3d.com/2019.3/Documentation/Manual/Packages.html)
> - [Creating custom packages](https://docs.unity3d.com/2019.1/Documentation/Manual/CustomPackages.html)
> - [Package manifest](https://docs.unity3d.com/2019.1/Documentation/Manual/upm-manifestPkg.html)
> - [Adding and removing packages](https://docs.unity3d.com/2019.1/Documentation/Manual/upm-ui-actions.html)
> - [Scoped package registries](https://docs.unity3d.com/2019.1/Documentation/Manual/upm-scoped.html)

## Installing from the registry

### OpenUPM

#### Register Your Package

1. Visit https://openupm.com/packages/add/
2. Submit your public Git repository URL
3. OpenUPM handles the rest automatically

> This package is available on [OpenUPM](https://openupm.com/packages/com.pokeum.upm/):
> 
> ```shell
> openupm add com.pokeum.upm
> ```

#### Scoped package registries

Add the following to your `Packages/manifest.json`:

```json
{
  "dependencies": {
    "com.pokeum.upm": "0.0.1"
  },
  "scopedRegistries": [
    {
      "name": "package.openupm.com",
      "url": "https://package.openupm.com",
      "scopes": [
        "com.pokeum.upm",
        "com.google.external-dependency-manager"
      ]
    }
  ]
}
```

<img src="/image/package-manager.png"  width="900">

## Installing from a Git URL

1. Open Package Manager
2. Click on the + icon on the top left corner of the "Package Manager" screen
3. Click on "Install package from git url..."
4. Paste: https://github.com/pokeum/upm-sample.git?path=Package

> [!NOTE]  
> You can’t specify Git dependencies in a `package.json` file because the Package Manager doesn’t support Git dependencies between packages.
> **It supports Git dependencies only for projects**, so you can declare Git dependencies only in the project’s `manifest.json` file.
>
> ```log
> Cannot perform upm operation: Unable to add package [https://github.com/pokeum/upm-sample.git?path=Package]:
>   Package com.pokeum.upm@https://github.com/pokeum/upm-sample.git?path=Package has invalid dependencies or related test packages:
>     com.google.external-dependency-manager (dependency): Package [com.google.external-dependency-manager@1.2.178] cannot be found [NotFound]
> UnityEditor.EditorApplication:Internal_CallUpdateFunctions () (at /Users/bokken/build/output/unity/unity/Editor/Mono/EditorApplication.cs:310)
> ```

### Accessing package assets 

```csharp
private static string GetAssetsPath()
{
  // https://docs.unity3d.com/ScriptReference/Application-dataPath.html
  // Unity Editor: <path to project folder>/Assets
  string path = Application.dataPath;

  try
  {
    // https://docs.unity3d.com/Manual/upm-assets.html
    string packagePath = Path.GetFullPath("Packages/<package-name>/Assets");
    if (Directory.Exists(packagePath))
    {
      path = packagePath;
    }
  }
  catch (Exception) { /* ignored */ }
        
  return path;
}
```


### Packages installed location

- Install a UPM package from a Git URL

  ```
  {PROJECT_PATH}/Library/PackageCache
  ```
