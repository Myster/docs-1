---
title: Pushing packages to the Built-In repository

---


:::success
**Supported packages**
The Octopus built-in repository has always supported NuGet packages, and since Octopus 3.3 it can [support many different types of packages](/docs/home/packaging-applications/supported-packages.md).
:::


We offer several ways to add/upload/push packages to the built-in feed:


- Using the Octopus web portal
- Using your build server
- Using Octo.exe
- Using the Octopus API (HTTP POST)
- Using NuGet.exe push
- Using npm.exe, grunt or gulp
- Using curl
- Security considerations

## Using the Octopus web portal


You can manually upload a package file from your local machine via the Octopus web portal via the�*Library > Packages*�tab by clicking the�*Upload package*�button.


![](/docs/images/3048094/3277775.png)

:::success
We generally recommend using a continuous integration/build server like [TeamCity](/docs/home/api-and-integration/teamcity.md), [Jenkins](/docs/home/api-and-integration/jenkins.md), [Bamboo](/docs/home/api-and-integration/bamboo.md) or [Team Foundation Server (TFS)](/docs/home/api-and-integration/team-foundation-server-(tfs).md) to build, test, package and automatically push your release packages into the Octopus Deploy built-in repository. See below for examples on doing this.
:::

:::hint
For pushing packages using the methods described below you'll need:

1. The URL to your Octopus Server
2. An [Octopus API key](/docs/home/how-to/how-to-create-an-api-key.md) with the required permissions (see [security considerations](/docs/home/packaging-applications/package-repositories/pushing-packages-to-the-built-in-repository.md))
:::

## Using your build server


We have built integrations/plugins/extensions for the most popular build servers. You can read more about [integrating Octopus Deploy with your build server](/docs/home/api-and-integration.md). In most cases you simply provide the build server with the URL to your Octopus Server and an�[Octopus API key](/docs/home/how-to/how-to-create-an-api-key.md) with the required permissions� (see [security considerations](/docs/home/packaging-applications/package-repositories/pushing-packages-to-the-built-in-repository.md)).

## Using Octo.exe


You can push one or more packages using Octo.exe, the command-line tool for Octopus Deploy. The example below will push `MyApp.Website.1.1.0.zip`�and `MyApp.Database.1.1.0.zip` to the built-in repository, automatically replacing existing packages if there are conflicts.

```powershell
C:\> Octo.exe push --package MyApp.Website.1.1.0.zip --package MyApp.Database.1.1.0.zip --replace-existing --server http://my.octopus.url --apiKey API-XXXXXXXXXXXXXXXX
```


For more information refer to�[Pushing packages with Octo.exe](/docs/home/api-and-integration/octo.exe-command-line/pushing-packages.md).

## Using the Octopus API (HTTP POST)


You can upload a package via the�[Octopus Deploy API](/docs/home/api-and-integration/octopus-rest-api.md)�-�`POST /api/packages/raw HTTP 1.1`.

- [C# example (LINQPad)](https://github.com/OctopusDeploy/OctopusDeploy-Api/blob/master/Octopus.Client/LINQPad/Push%20Package%20to%20Built-In%20Repository.linq)
- [PowerShell example](https://github.com/OctopusDeploy/OctopusDeploy-Api/blob/master/REST/PowerShell/Packages/PushPackage.ps1)


## Using NuGet.exe push


To push a package using�`NuGet.exe`�you'll need a the URL for the Octopus NuGet feed to use with your build server or�`NuGet.exe`. To find this, open the�*Library > Packages*�tab of the Octopus web portal.�The screen shows an example command-line that can be used to push packages to the feed using�[NuGet.exe](http://docs.nuget.org/docs/start-here/installing-nuget). You'll need to supply the NuGet package file (`.nupkg`) and an�[Octopus API key](/docs/home/how-to/how-to-create-an-api-key.md).


![](/docs/images/3048094/3277775.png)




:::success
If you're using a continuous integration server like TeamCity to produce packages you can use their built-in NuGet Push step. Supply the Octopus NuGet feed URL shown above and an�[Octopus API key](/docs/home/how-to/how-to-create-an-api-key.md)�when prompted for the feed details.
:::

## Using npm.exe, grunt or gulp


You can upload packages using npm.exe or using our grunt or gulp tasks. Take a look at our [guide for packaging and deploying Node.js applications using Octopus Deploy](/docs/home/guides/node-on-nix-deployments.md).

## Using curl


You can upload packages using `curl`. Like all of the other examples you will need your Octopus Server URL and an API Key. This will perform a POST uploading the file contents as multi-part form data.

```powershell
curl -X POST https://demo.octopus.com/api/packages/raw -H "X-Octopus-ApiKey: API-YOURAPIKEY" -F "data=@Demo.1.0.0.zip"
```

:::success
You may need to use the `-k` argument if you are using an untrusted connection.
:::

## Security considerations


To add a new package to the built-in feed requires the�`BuiltInFeedPush`�permission.�To delete a package, or replace an existing package requires the�`BuiltInFeedAdminister`�permission.


For your convenience Octopus Deploy provides a built-in role called�**Package Publisher**�that has been granted the�`BuiltInFeedPush`�permission.

:::hint
**Consider using a Service Account**
Instead of using your own API key, consider using a [Service Account](/docs/home/administration/managing-users-and-teams/service-accounts.md) to provide limited permissions since packages will normally be pushed by an automated service like your build server. Service Accounts are API-only accounts that cannot be used sign in to the Octopus Deploy web portal.
:::

:::hint
**Using automatic release creation?**
If you are using [automatic release creation](/docs/home/deploying-applications/automatic-release-creation.md) you will also require the permissions to create a release for all of the relevant projects in the required environments. To diagnose issues with pushing packages used for automatic release creation follow the troubleshooting guide on the [automatic release creation](/docs/home/deploying-applications/automatic-release-creation.md) page.
:::