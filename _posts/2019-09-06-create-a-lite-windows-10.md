---
title: Create a Lite Windows 10
tags: Windows
---

Since I usually use macOS, in some specific situations, such as using the full version of the Office, I need to temporarily use the Windows. My solution is to use a Windows virtual machine, but the image is one of the most simplified version.

## Tools

We need an official Windows 10 image (you can find the official download URL from [TechBench by WZT](https://tb.rg-adguard.net/public.php)) and several other tools to rebuild that image.

- [Windows 10](https://tb.rg-adguard.net/public.php)
- [UltraISO](https://www.ultraiso.com)
- [Dism++](http://www.chuyu.me/en/index.html)
- [MSMG Toolkit](https://msmgtoolkit.in/download.html)

![Windows 10 image and other tools]({{ site.baseurl }}/assets/images/Windows 10 image and other tools.png)

## Rebuild Windows 10 Image

Find a Windows PC to complete the following steps.

### Remove Redundant Windows Versions

If the Windows image contains multiple versions, we can reduce the size of the image by removing the extra versions.

1. Open the Windows ISO image from UltraISO using `File -> Open...`
1. Extract `install.win` from `sources` folder using `Extract To...`

    ![Extract install image with UltraISO]({{ site.baseurl }}/assets/images/Extract install image with UltraISO.png)

1. Open `install.win` from Dism++ using `File -> Open Image File`
1. Extract the smallest image using `Export image` (Choose `Max` in `Save as type`)

    ![Extract install image with Dism++]({{ site.baseurl }}/assets/images/Extract install image with Dism++.png)

1. Replace the original `install.win` with the new extracted small installation image in UltraISO using `Actions -> Add Files...`
1. Save the Windows image using `File -> Save`

The new image (the first) is a few hundred megabytes smaller than the original image.

![Size comparison after removing redundant Windows versions]({{ site.baseurl }}/assets/images/Size comparison after removing redundant Windows versions.png)

### Remove Redundant Windows Components

Next, we need to remove the extra Windows components to further reduce the storage after the operating system is installed.

1. Extract downloaded ToolKit archive file
1. Copy your new Windows ISO image to ToolKit's `ISO` folder
1. Run `Start.cmd` as administrator
1. Agree to ToolKit's EULA by pressing `A` key
1. Press `Enter` key to continue
1. Extract the source image using `Source -> Extract Source from DVD ISO Image` menu
1. Select the source image using `Source -> Select Source from <DVD> Folder` menu
1. Cleanup the source image using `Apply -> Cleanup Source Images` menu
1. Remove all required Windows components using `Remove -> Remove Windows Components` menu (This could take hours depends on how many components you want to remove)
1. Cleanup the source image using `Apply -> Cleanup Source Images` menu
1. Apply & save changes to source image using `Apply -> Apply & Save Changes to Source Images` menu
1. Rebuild source image using `Apply -> Rebuild Source Images` menu
1. Export source image to a ISO image `Target -> Make a DVD ISO Image` menu

You will find the new ISO image (the second) in ToolKit's `ISO` folder which is much smaller than the original image (the third).

![Size comparison after removing redundant Windows components]({{ site.baseurl }}/assets/images/Size comparison after removing redundant Windows components.png)

## Test in VM

I create two virtual machines with the original image and the new lite image, then compare their resource usage. I choose `Windows 10 Home N` for the original version of Windows 10 virtual machine which is the same Windows version in the lite image.

![Choose Windows version during installation]({{ site.baseurl }}/assets/images/Choose Windows version during installation.png)

### Result

We can see from the picture below that virtual machines created with the lite image take up less storage and use less memory at runtime.

![Resource usage comparison between original Windows and lite Windows]({{ site.baseurl }}/assets/images/Resource usage comparison between original Windows and lite Windows.png)

This is mainly because the system only contains the necessary system applications.

![Apps number comparison between original Windows and lite Windows]({{ site.baseurl }}/assets/images/Apps number comparison between original Windows and lite Windows.png)

As you can see from the comparison, the lite virtual machine is indeed lighter and certainly faster.
