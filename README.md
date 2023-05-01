<a href="https://aimeos.org/">
    <img src="https://github.com/Rafee-M/OnePlus-9RT-Root/blob/main/images/android-root.png" align="right" height="140" />
</a>

# OnePlus 9RT Root Guide
This guide contains all the steps to root your OnePlus 9RT and unlock it's potential!

## Table Of Content

- [Bootloader Unlock](bootloader-unlock)
    - [Requirements](#requirements)
    - [Setting up adb](#ddev)
    - [Setting up adb](#ter-extension)
    - [Fastboot](#fastboot)
- [TYPO3 setup](#typo3-setup)
    - [Database setup](#database-setup)
    - [Security](#security)
- [Page setup](#page-setup)
    - [Download the Aimeos Page Tree t3d file](#download-the-aimeos-page-tree-t3d-file)
    - [Go to the Import View](#go-to-the-import-view)
    - [Upload the page tree file](#upload-the-page-tree-file)
    - [Go to the import view](#go-to-the-import-view)
    - [Import the page tree](#import-the-page-tree)
    - [SEO-friendly URLs](#seo-friendly-urls)
- [License](#license)
- [Links](#links)

## Bootloader Unlock

We will have to unlock the bootloader of the phone, only then we can proceed to rooting it.

**WARNNG:** This will DELETE ALL THE DATA from your phone, so create a backup of anything important

### Requirements

Download the following items: <img src="https://github.com/Rafee-M/OnePlus-9RT-Root/blob/main/images/windows-update.png" align="right" height="360"/>

1. [SDK Platform Tools](https://developer.android.com/tools/releases/platform-tools) 

2. Android Bootloader Interface from Windows Update -> Optional Updates -> Driver Updates
_Note:_ You will find this update when you're at the [Fastboot Step - 2](#fastboot)

### Setting Up Your Phone

1. Enabling Developer Options:
- Go to Settings
- Go to About Phone
- Go to Version
- Tap on Build Number 8 times

2. OEM Unlock and USB Debugging:
- Go to Additional Settings
- Go to Developer Option
- Find and enable USB Debugging
- Find and enable OEM Unlock

### Setting up adb

1. Connect your phone to the computer
2. Go to the SDK Platform Tools folder and in the address bar type "cmd"
3. In the command prompmt type `adb devices`
4. Open your phone and press allow on the USB Debugging prompt and it should look like this now 

<img src="https://github.com/Rafee-M/OnePlus-9RT-Root/blob/main/images/adb-devices.png">

### Fastboot

1. To go into Fastboot mode type in the same cmd promt `adb reboot bootloader`
2. You should see your phone go into a different menu
3. 

For new TYPO3 installations, there is a 1-click [Aimeos distribution](https://typo3.org/extensions/repository/view/aimeos_dist) available, too. Choose the Aimeos distribution from the list of available distributions in the Extension Manager and you will get a completely set up shop system including demo data for a quick start.

## TYPO3 Setup

Setup TYPO3 by creating a `FIRST_INSTALL` file in the `./public` directory:

```bash
touch public/FIRST_INSTALL
```

Open the URL of your installation in the browser and follow the steps in the TYPO3 setup scripts.

### Database Setup

If you use MySQL < 5.7.8, you have to use `utf8` and `utf8_unicode_ci` instead because those MySQL versions can't handle the long indexes created by `utf8mb4` (up to four bytes per character) and you will get errors like

```
1071 Specified key was too long; max key length is 767 bytes
```

To avoid that, change your database settings in your `./typo3conf/LocalConfiguration.php` to:

```php
    'DB' => [
        'Connections' => [
            'Default' => [
                'tableoptions' => [
                    'charset' => 'utf8',
                    'collate' => 'utf8_unicode_ci',
                ],
                // ...
            ],
        ],
    ],
```

### Security

Since **TYPO3 9.5.14+** implements **SameSite cookie handling** and restricts when browsers send cookies to your site. This is a problem when customers are redirected from external payment provider domain. Then, there's no session available on the confirmation page. To circumvent that problem, you need to set the configuration option `cookieSameSite` to `none` in your `./typo3conf/LocalConfiguration.php`:

```php
    'FE' => [
        'cookieSameSite' => 'none'
    ]
```

## Site Setup

TYPO3 10+ requires a site configuration which you have to add in "Site Management" > "Sites" available in the left navigation. When creating a root page (a page with a globe icon), a basic site configuration is automatically created (see below at [Go to the Import View](#go-to-the-import-view)).

## Page Setup

### Download the Aimeos Page Tree t3d file

The page setup for an Aimeos web shop is easy, if you import the example page tree for TYPO3 10/11. You can download the version you need from here:

* [23.4+ page tree](https://aimeos.org/fileadmin/download/Aimeos-pages_2023.04.t3d) and later
* [22.10 page tree](https://aimeos.org/fileadmin/download/Aimeos-pages_2022.10.t3d)
* [21.10 page tree](https://aimeos.org/fileadmin/download/Aimeos-pages_21.10.t3d)

**Note:** The Aimeos layout expects [Bootstrap](https://getbootstrap.com) providing the grid layout!

In order to upload and install the file, follow the following steps:

### Go to the Import View

**Note:** It is recommended to import the Aimeos page tree to a page that is defined as "root page". To create a root page, simply create a new page and, in the "Edit page properties", activate the "Use as Root Page" option under "Behaviour". The icon of the root page will change to a globe. This will also create a basic site configuration. Don't forget to also create a typoscript root template and include the bootstrap templates with it!

![Create a root page](https://user-images.githubusercontent.com/213803/211549273-1d3883dd-710c-4e27-8dbb-3de6e45680d7.jpg)

* In "Web::Page", right-click on the root page (the one with the globe)
* Click on "More options..."
* Click on "Import"

![Go to the import view](https://user-images.githubusercontent.com/213803/211550212-df6daa73-74cd-459e-8d25-a56c413c175d.jpg)

### Upload the page tree file

* In the page import dialog
* Select the "Upload" tab (2nd one)
* Click on the "Select" dialog
* Choose the T3D file you've downloaded
* Press the "Upload files" button

![Upload the page tree file](https://user-images.githubusercontent.com/8647429/212347778-17238e05-7494-4413-adb3-a54b2b524e05.png)

### Import the page tree

* In Import / Export view
* Select the uploaded file from the drop-down menu
* Click on the "Preview" button
* The pages that will be imported are shown below
* Click on the "Import" button that has appeared
* Confirm to import the pages

![Import the uploaded page tree file](https://user-images.githubusercontent.com/8647429/212348040-c3e10b60-5579-4d1b-becc-72548826c6db.png)

Now you have a new page "Shop" in your page tree including all required sub-pages.

### SEO-friendly URLs

TYPO3 9.5 and later can create SEO friendly URLs if you add the rules to the site config:
[https://aimeos.org/docs/latest/typo3/setup/#seo-urls](https://aimeos.org/docs/latest/typo3/setup/#seo-urls)

## License

The Aimeos TYPO3 extension is licensed under the terms of the GPL Open Source
license and is available for free.

## Links

* [Web site](https://aimeos.org/integrations/typo3-shop-extension/)
* [Documentation](https://aimeos.org/docs/TYPO3)
* [Forum](https://aimeos.org/help/typo3-extension-f16/)
* [Issue tracker](https://github.com/aimeos/aimeos-typo3/issues)
* [Source code](https://github.com/aimeos/aimeos-typo3)
