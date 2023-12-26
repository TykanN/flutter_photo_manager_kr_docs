<!-- Copyright 2018 The FlutterCandies author. All rights reserved.
Use of this source code is governed by an Apache license
that can be found in the LICENSE file. -->

# photo_manager

[English](README.md) | [中文](README-ZH.md) | 한국어

[![pub package](https://img.shields.io/pub/v/photo_manager?label=stable)][pub package]
[![pub pre-release package](https://img.shields.io/pub/v/photo_manager?color=9d00ff&include_prereleases&label=dev)](https://pub.dev/packages/photo_manager)
[![Build status](https://img.shields.io/github/actions/workflow/status/fluttercandies/flutter_photo_manager/runnable.yml?branch=main&label=CI&logo=github&style=flat-square)](https://github.com/fluttercandies/flutter_photo_manager/actions/workflows/runnable.yml)
[![GitHub license](https://img.shields.io/github/license/fluttercandies/flutter_photo_manager)](https://github.com/fluttercandies/flutter_photo_manager/blob/main/LICENSE)

[![GitHub stars](https://img.shields.io/github/stars/fluttercandies/flutter_photo_manager?logo=github&style=flat-square)](https://github.com/fluttercandies/flutter_photo_manager/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/fluttercandies/flutter_photo_manager?logo=github&style=flat-square)](https://github.com/fluttercandies/flutter_photo_manager/network)
[![Awesome Flutter](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/Solido/awesome-flutter)
<a target="_blank" href="https://jq.qq.com/?_wv=1027&k=5bcc0gy"><img border="0" src="https://pub.idqqimg.com/wpa/images/group.png" alt="FlutterCandies" title="FlutterCandies"></a>

UI 작업없이 에셋(Asset)을 관리할 수 있는 추상 API 플러그인으로,
Android, iOS 및 macOS에서 에셋(이미지/비디오/오디오)에 접근할 수 있습니다.

## 이 플러그인을 사용하는 프로젝트

| name                 | pub                                                                                                                | github                                                                                                                                                                  |
| :------------------- | :----------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| wechat_assets_picker | [![pub package](https://img.shields.io/pub/v/wechat_assets_picker)](https://pub.dev/packages/wechat_assets_picker) | [![star](https://img.shields.io/github/stars/fluttercandies/flutter_wechat_assets_picker?style=social)](https://github.com/fluttercandies/flutter_wechat_assets_picker) |
| wechat_camera_picker | [![pub package](https://img.shields.io/pub/v/wechat_camera_picker)](https://pub.dev/packages/wechat_camera_picker) | [![star](https://img.shields.io/github/stars/fluttercandies/flutter_wechat_camera_picker?style=social)](https://github.com/fluttercandies/flutter_wechat_camera_picker) |

## 이 플러그인에 대한 아티클

- [Hard to manage media with Flutter? Try photo_manager, the all-in-one solution](https://medium.flutter.cn/hard-to-manage-media-with-flutter-try-photo-manager-the-all-in-one-solution-5188599e4cf)

## 마이그레이션 가이드

메이저 버전이 변경되는 업그레이드를 하는 경우,
자세한 내용은 [마이그레이션 문서](MIGRATION_GUIDE.md)를 확인하세요.

<details>
  <summary>목차</summary>

<!-- TOC -->

- [photo\_manager](#photo_manager)
  - [이 플러그인을 사용하는 프로젝트](#이-플러그인을-사용하는-프로젝트)
  - [이 플러그인에 대한 아티클](#이-플러그인에-대한-아티클)
  - [마이그레이션 가이드](#마이그레이션-가이드)
  - [자주 묻는 질문](#자주-묻는-질문)
  - [준비 단계](#준비-단계)
    - [pubspec.yaml에 플러그인 의존성 추가](#pubspecyaml에-플러그인-의존성-추가)
    - [프로젝트에 Import 하기](#프로젝트에-import-하기)
    - [네이티브 플랫폼 구성 설정](#네이티브-플랫폼-구성-설정)
      - [Android 구성 설정](#android-구성-설정)
        - [Kotlin, Gradle, AGP](#kotlin-gradle-agp)
        - [Android 10 (Q, 29)](#android-10-q-29)
        - [Glide](#glide)
      - [iOS 구성 설정](#ios-구성-설정)
  - [사용법](#사용법)
    - [권한 요청](#권한-요청)
      - [Limited entities access](#limited-entities-access)
        - [Limited entities access on iOS](#limited-entities-access-on-ios)
        - [Limited entities access on android](#limited-entities-access-on-android)
    - [Get albums/folders (`AssetPathEntity`)](#get-albumsfolders-assetpathentity)
      - [Params of `getAssetPathList`](#params-of-getassetpathlist)
      - [PMPathFilterOption](#pmpathfilteroption)
    - [Get assets (`AssetEntity`)](#get-assets-assetentity)
      - [From `AssetPathEntity`](#from-assetpathentity)
      - [From `PhotoManager` (Since 2.6)](#from-photomanager-since-26)
      - [From ID](#from-id)
      - [From raw data](#from-raw-data)
      - [From iCloud](#from-icloud)
      - [Display assets](#display-assets)
      - [Obtain "Live Photos"](#obtain-live-photos)
        - [Filtering only "Live Photos"](#filtering-only-live-photos)
        - [Obtain the video from "Live Photos"](#obtain-the-video-from-live-photos)
      - [Limitations](#limitations)
        - [Android 10 media location permission](#android-10-media-location-permission)
        - [Usage of the original data](#usage-of-the-original-data)
        - [Long retrieving duration with file on iOS](#long-retrieving-duration-with-file-on-ios)
    - [Entities change notify](#entities-change-notify)
  - [Filtering](#filtering)
    - [FilterOptionGroup](#filteroptiongroup)
    - [CustomFilter](#customfilter)
      - [Advanced CustomFilter](#advanced-customfilter)
      - [Classes explanations](#classes-explanations)
  - [Cache mechanism](#cache-mechanism)
    - [Cache on Android](#cache-on-android)
    - [Cache on iOS](#cache-on-ios)
    - [Clear caches](#clear-caches)
  - [Native extra configs](#native-extra-configs)
    - [Android extra configs](#android-extra-configs)
      - [Glide issues](#glide-issues)
      - [Android 14 (API 34) extra configs](#android-14-api-34-extra-configs)
      - [Android 13 (API 33) extra configs](#android-13-api-33-extra-configs)
    - [iOS extra configs](#ios-extra-configs)
      - [Localized system albums name](#localized-system-albums-name)
    - [Experimental features](#experimental-features)
      - [Preload thumbnails](#preload-thumbnails)
      - [Delete entities](#delete-entities)
      - [Copy an entity](#copy-an-entity)
      - [Features for Android only](#features-for-android-only)
        - [Move an entity to another album](#move-an-entity-to-another-album)
        - [Remove all non-exist entities](#remove-all-non-exist-entities)
        - [Move entities to trash](#move-entities-to-trash)
      - [Features for iOS or macOS](#features-for-ios-or-macos)
        - [Create a folder](#create-a-folder)
        - [Create an album](#create-an-album)
        - [Remove the entity entry from the album](#remove-the-entity-entry-from-the-album)
        - [Delete `AssetPathEntity`](#delete-assetpathentity)

</details>

## 자주 묻는 질문

빌드 오류, 런타임 예외 등 일반적으로 자주 발생하는 문제에 대해서는
[GitHub issues][]에서 먼저 검색하세요.

## 준비 단계

### pubspec.yaml에 플러그인 의존성 추가

pubspec에 의존성을 추가하는 2가지 방법:

- **(권장)** 터미널에서 `flutter pub add photo_manager` 실행.
- `pubspec.yaml`파일의 `dependencies` 부분에 플러그인 참조 문자열 추가:

```yaml
dependencies:
  photo_manager: $latest_version
```

가장 최신 버전은 다음과 같습니다:
[![pub package](https://img.shields.io/pub/v/photo_manager.svg)][pub package]

### 프로젝트에 Import 하기

```dart
import 'package:photo_manager/photo_manager.dart';
```

### 네이티브 플랫폼 구성 설정

각 플랫폼별 최소 버전:
**Android 16, iOS 9.0, macOS 10.15**.

- Android: [Android 구성 설정](#android-config-preparation).
- iOS: [iOS 구성 설정](#ios-config-preparation).
- macOS: iOS와 거의 동일합니다.

#### Android 구성 설정

##### Kotlin, Gradle, AGP

이 플러그인은 **Kotlin `1.7.22`** 버전을 사용합니다.
프로젝트가 위 버전보다 낮은 Kotlin/Gradle/AGP 버전을 사용하는 경우,
최신 버전으로 업그레이드 하세요.

상세 부연 설명:

- Gradle version (`gradle-wrapper.properties`)을
  `7.5.1` 또는 그 이상 버전으로 업그레이드 하세요.
- Kotlin version (`ext.kotlin_version`)을
  `1.7.22` 또는 그 이상 버전으로 업그레이드 하세요.
- AGP version (`com.android.tools.build:gradle`)을
  `7.2.2` 또는 그 이상 버전으로 업그레이드 하세요.

##### Android 10 (Q, 29)

_`compileSdkVersion` 또는 `targetSdkVersion`이 29가 아닌 경우,
이 부분은 건너 뛰어도 됩니다._

Android 10에서 **Scoped Storage** 가 도입되었으며,
원본 리소스 파일에 파일 경로를 통해 직접 액세스할 수 없습니다.

`compileSdkVersion` 또는 `targetSdkVersion`이 `29`인 경우,
리소스에 이용하기 위해 `AndroidManifest.xml`에 `android:requestLegacyExternalStorage="true"`을
추가하는 것을 고려해볼 수 있습니다.:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.fluttercandies.photo_manager_example">

    <application
        android:label="photo_manager_example"
        android:icon="@mipmap/ic_launcher"
        android:requestLegacyExternalStorage="true">
    </application>
</manifest>
```

**주의: 이 플래그 사용이 설정된 앱은 Google Play에서 반려될 수 있습니다.**

해당 부분은 필수 사항이 아니며, 플러그인 여전히 캐싱파일과 함께 동작합니다.
하지만 캐시는 스스로 제어해야 합니다. 가장 좋은 방법은 앱이 시작될 때 마다
`PhotoManager.clearFileCache()`을 호출하여 파일 캐시를 지우는 방법입니다.

##### Glide

안드로이드에서 썸네일을 생성하기 위해 [Glide][]를 사용합니다.

Glide 관련 표시와 함께 경고 로그가 발생한다면,
프로젝트에서 `AppGlideModule`의 구현이 필요합니다.
구현을 위해 [Generated API][]을 참고하십시오.

#### iOS 구성 설정

`ios/Runner/Info.plist` 에서
`NSPhotoLibraryUsageDescription` key-value를 추가하세요:

```plist
<key>NSPhotoLibraryUsageDescription</key>
<string>사진 등록을 위해 앨범 접근 권한이 필요합니다.</string>
```

iOS 11 이상의 사진 라이브러리에 쓰기 액세스 권한만 부여하고 싶다면,
`ios/Runner/Info.plist` 에서
`NSPhotoLibraryAddUsageDescription` key-value를 추가하세요:
`NSPhotoLibraryUsageDescription` 설정과 동일합니다.

![permissions in Xcode](https://raw.githubusercontent.com/CaiJingLong/some_asset/master/flutter_photo2.png)

## 사용법

### 권한 요청

대부분의 API는 권한이 허락된 경우에만 사용할 수 있습니다.

```dart
final PermissionState ps = await PhotoManager.requestPermissionExtend(); // the method can use optional param `permission`.
if (ps.isAuth) {
  // 권한이 있는 경우
  // 에셋에 엑세스 가능
} else if (ps.hasAccess) {
  // 액세스는 계속되지만 표시되는 에셋은 사용자의 선택에 따라 달라집니다.
} else {
  // 제한(iOS) 또는 허용하지 않음, 정확한 확인을 위해 `==` 연산자를 사용할 수 있습니다.
  // 추가 설정 단계를 위해 `PhotoManager.openSetting()`를 호출하여 설정 창을 열 수 있습니다.
}
```

앱에 권한이 허용된 것을 보장할 수 있는 경우, 권한 확인도 무시할 수 있습니다:

```dart
PhotoManager.setIgnorePermissionCheck(true);
```

For background processing (such as when the app is not in the foreground),
ignore permissions check would be proper solution.

#### Limited entities access

##### Limited entities access on iOS

With iOS 14 released, Apple brought a "Limited Photos Library" permission
(`PermissionState.limited`) to iOS.
The `PhotoManager.requestPermissionExtend()` method will return `PermissionState`.
See [PHAuthorizationStatus][] for more detail.

To reselect accessible entities for the app,
use `PhotoManager.presentLimited()` to call the modal of
accessible entities' management.
This method only available for iOS 14+ and when the permission state
is limited (`PermissionState.limited`).

##### Limited entities access on android

Android 14 (API 34) has also introduced the concept of limited assets similar to iOS.

However, there is a slight difference in behavior (based on the emulator):
On Android, the access permission to a certain resource cannot be revoked once it is granted,
even if it hasn't been selected when using `presentLimited` in future actions.

### Get albums/folders (`AssetPathEntity`)

Albums or folders are abstracted as the [`AssetPathEntity`][] class.
It represents a bucket in the `MediaStore` on Android,
and the `PHAssetCollection` object on iOS/macOS.
To get all of them:

```dart
final List<AssetPathEntity> paths = await PhotoManager.getAssetPathList();
```

See [`getAssetPathList`][] for more detail.

#### Params of `getAssetPathList`

| Name             | Description                                                                                                                    | Default value           |
| :--------------- | ------------------------------------------------------------------------------------------------------------------------------ | ----------------------- |
| hasAll           | Set to true when you need an album containing all assets.                                                                      | true                    |
| onlyAll          | Use this property if you only need one album containing all assets.                                                            | false                   |
| type             | Type of media resource (video, image, audio)                                                                                   | RequestType.common      |
| filterOption     | Used to filter resource files, see [Filtering](#filtering) for details                                                         | FilterOptionGroup()     |
| pathFilterOption | Only valid for iOS and macOS, for the corresponding album type. See [PMPathFilterOption](#pmpathfilteroption) for more detail. | Include all by default. |

#### PMPathFilterOption

Provide since 2.7.0, currently only valid for iOS and macOS.

```dart
final List<PMDarwinAssetCollectionType> pathTypeList = []; // use your need type
final List<PMDarwinAssetCollectionSubtype> pathSubTypeList = []; // use your need type
final darwinPathFilterOption = PMDarwinPathFilter(
      type: pathTypeList,
      subType: pathSubTypeList,
    );
PMPathFilter pathFilter = PMPathFilter();
```

The `PMDarwinAssetCollectionType` has a one-to-one correspondence with [PHAssetCollectionType | Apple Developer Documentation](https://developer.apple.com/documentation/photokit/phassetcollectiontype?language=objc).

The `PMDarwinAssetCollectionSubtype` has a one-to-one correspondence with [PHAssetCollectionSubType | Apple Developer Documentation](https://developer.apple.com/documentation/photokit/phassetcollectionsubtype?language=objc).

### Get assets (`AssetEntity`)

Assets (images/videos/audios) are abstracted as the [`AssetEntity`][] class.
It represents a series of fields with `MediaStore` on Android,
and the `PHAsset` object on iOS/macOS.

#### From `AssetPathEntity`

You can use [the pagination method][`getAssetListPaged`]:

```dart
final List<AssetEntity> entities = await path.getAssetListPaged(page: 0, size: 80);
```

Or use [the range method][`getAssetListRange`]:

```dart
final List<AssetEntity> entities = await path.getAssetListRange(start: 0, end: 80);
```

#### From `PhotoManager` (Since 2.6)

First, You need get count of assets:

```dart
final int count = await PhotoManager.getAssetCount();
```

Then, you can use [the pagination method][`PhotoManager.getAssetListPaged`]:

```dart
final List<AssetEntity> entities = await PhotoManager.getAssetListPaged(page: 0, pageCount: 80);
```

Or use [the range method][`PhotoManager.getAssetListRange`]:

```dart
final List<AssetEntity> entities = await PhotoManager.getAssetListRange(start: 0, end: 80);
```

**Note:** `page` and `start` starts from **0**.

#### From ID

The ID concept represents:

- The ID field of the `MediaStore` on Android.
- The `localIdentifier` field of the `PHAsset` on iOS.

You can store the ID if you want to implement features
that's related to persistent selections.
Use [`AssetEntity.fromId`][] to retrieve the entity
once you persist an ID.

```dart
final AssetEntity? asset = await AssetEntity.fromId(id);
```

Be aware that the created asset might have
limited access or got deleted in anytime,
so the result might be null.

#### From raw data

You can create an entity from raw data,
such as downloaded images, recorded videos, etc.
The created entity will show as a corresponding resource
on your device's gallery app.

```dart
final Uint8List rawData = yourRawData;

// Save an image to an entity from `Uint8List`.
final AssetEntity? entity = await PhotoManager.editor.saveImage(
  rawData,
  title: 'write_your_own_title.jpg', // Affects EXIF reading.
);

// Save an existed image to an entity from it's path.
final AssetEntity? imageEntityWithPath = await PhotoManager.editor.saveImageWithPath(
  path, // Use the absolute path of your source file, it's more like a copy method.
  title: 'same_as_above.jpg',
);

// Save a video entity from `File`.
final File videoFile = File('path/to/your/video.mp4');
final AssetEntity? videoEntity = await PhotoManager.editor.saveVideo(
  videoFile, // You can check whether the file is exist for better test coverage.
  title: 'write_your_own_title.mp4',
);

// [iOS only] Save a live photo from image and video `File`.
// This only works when both image and video file were part of same live photo.
final File imageFile = File('path/to/your/live_photo.heic');
final File videoFile = File('path/to/your/live_video.mp4');
final AssetEntity? entity = await PhotoManager.editor.darwin.saveLivePhoto(
  imageFile: imageFile,
  videoFile: videoFile,
  title: 'write_your_own_title.heic',
);
```

Be aware that the created asset might have
limited access or got deleted in anytime,
so the result might be null.

#### From iCloud

Resources might be saved only on iCloud to save disk space.
When retrieving file from iCloud, the speed is depended on the network condition,
which might be very slow that makes users feel anxious.
To provide a responsive user interface, you can use `PMProgressHandler`
to retrieve the progress when load a file.

The preferred implementation would be the [`LocallyAvailableBuilder`][]
in the `wechat_asset_picker` package, which provides a progress indicator
when the file is downloading.

#### Display assets

> Starts from v3.0.0, `AssetEntityImage` and `AssetEntityImageProvider`
> are provided in the
> [`photo_manager_image_provider`][photo_manager_image_provider] package.

Use the `AssetEntityImage` widget or the `AssetEntityImageProvider` to display assets:

```dart
import 'package:photo_manager_image_provider/photo_manager_image_provider.dart';

final Widget image = AssetEntityImage(
  yourAssetEntity,
  isOriginal: false, // Defaults to `true`.
  thumbnailSize: const ThumbnailSize.square(200), // Preferred value.
  thumbnailFormat: ThumbnailFormat.jpeg, // Defaults to `jpeg`.
);

final Widget imageFromProvider = Image(
  image: AssetEntityImageProvider(
    yourAssetEntity,
    isOriginal: false,
    thumbnailSize: const ThumbnailSize.square(200),
    thumbnailFormat: ThumbnailFormat.jpeg,
  ),
);
```

#### Obtain "Live Photos"

This plugin supports obtain live photos and filtering them:

##### Filtering only "Live Photos"

This is supported when filtering only image.

```dart
final List<AssetPathEntity> paths = await PhotoManager.getAssetPathList(
  type: RequestType.image,
  filterOption: FilterOptionGroup(onlyLivePhotos: true),
);
```

##### Obtain the video from "Live Photos"

```dart
final AssetEntity entity = livePhotoEntity;
final String? mediaUrl = await entity.getMediaUrl();
final File? imageFile = await entity.file;
final File? videoFile = await entity.fileWithSubtype;
final File? originImageFile = await entity.originFile;
final File? originVideoFile = await entity.originFileWithSubtype;
```

#### Limitations

##### Android 10 media location permission

Due to the privacy policy issues on Android 10,
it is necessary to grant the location permission
to obtain the original data with the location info
and the EXIF metadata.

If you want to use the location permission,
add the `ACCESS_MEDIA_LOCATION` permission to your manifest.

##### Usage of the original data

The `originFile` and `originBytes` getter
will return the original data of an entity.
However, there are some cases that the original data is invalid in Flutter.
Here are some common cases:

- HEIC files are not fully supported across platforms. We suggest you to
  upload the JPEG file (`.file` if the file is HEIC) in order to keep
  a consistent behavior between multiple platforms.
  See [flutter/flutter#20522][] for more detail.
- Videos will only be obtained in the original format,
  not the exported/composited format, which might cause
  some behavior difference when playing videos.

##### Long retrieving duration with file on iOS

There are several I/O methods in this library targeting `AssetEntity`,
typically they are:

- All methods named with `file`.
- `AssetEntity.originBytes`.

File retrieving and caches are limited by the sandbox mechanism on iOS.
An existing `PHAsset` doesn't mean the file located on the device.
In generally, a `PHAsset` will have three status:

- `isLocallyAvailable` equals `true`, **also cached**: Available for obtain.
- `isLocallyAvailable` equals `true`, **but not cached**: When you call I/O methods,
  the resource will first cache into the sandbox, then available for obtain.
- `isLocallyAvailable` equals `false`: Typically this means the asset exists,
  but it's saved only on iCloud, or some videos that not exported yet.
  In this case, the best practise is to use the `PMProgressHandler`
  to provide a responsive user interface.

### Entities change notify

Plugin will post entities change events from native,
but they will include different contents.
See [the `logs` folder](log) for more recorded logs.

To register a callback for these events, use
`PhotoManager.addChangeCallback` to add a callback,
and use `PhotoManager.removeChangeCallback` to remove the callback,
just like `addListener` and `removeListener` methods.

After you added/removed callbacks, you can call
`PhotoManager.startChangeNotify` method to enable to notify,
and `PhotoManager.stopChangeNotify` method to stop notify.

```dart
import 'package:flutter/services.dart';

void changeNotify(MethodCall call) {
  // Your custom callback.
}

/// Register your callback.
PhotoManager.addChangeCallback(changeNotify);

/// Enable change notify.
PhotoManager.startChangeNotify();

/// Remove your callback.
PhotoManager.removeChangeCallback(changeNotify);

/// Disable change notify.
PhotoManager.stopChangeNotify();
```

## Filtering

Filtering assets are also supported by the plugin.
Below methods have the `filterOption` argument to define the conditions when obtaining assets.

- PhotoManager
  - getAssetPathList (Accessible with `AssetPathEntity.filterOption`)
  - getAssetCount
  - getAssetListRange
  - getAssetListPaged
- AssetPathEntity
  - constructor (not recommended to use directly)
  - fromId
  - obtainPathFromProperties (not recommended to use directly)

There are two implementations of filters:

- [FilterOptionGroup](#filteroptiongroup)
- [CustomFilter](#customfilter)

### FilterOptionGroup

The `FilterOptionGroup` is the only implementation before v2.6.0.

```dart
final FilterOptionGroup filterOption = FilterOptionGroup(
  imageOption: FilterOption(
    sizeConstraint: SizeConstraint(
      maxWidth: 10000,
      maxHeight: 10000,
      minWidth: 100,
      minHeight: 100,
      ignoreSize: false,
    ),
  ),
  videoOption: FilterOption(
    durationConstraint: DurationConstraint(
      min: Duration(seconds: 1),
      max: Duration(seconds: 30),
      allowNullable: false,
    ),
  ),
  createTimeCondition: DateTimeCondition(
    min: DateTime(2020, 1, 1),
    max: DateTime(2020, 12, 31),
  ),
  orders: [
    OrderOption(
      type: OrderOptionType.createDate,
      asc: false,
    ),
  ],
  /// other options
);
```

### CustomFilter

**NOTE:** The `CustomFilter` is still a newbie of the plugin that introduced in v2.6.0,
please help us to improve it by submitting issues if you've met any related problems.

`CustomFilter` provides flexible filtering conditions, and it's targeting host platforms.
The usages of the filter are more look close to native integrations,
which is a `SQL-like` filter.

The SQL columns name on Android and Darwin (iOS/macOS) are different,
you'll need to use `CustomColumns.base`, `CustomColumns.android` and `CustomColumns.darwin`
to get the columns name correctly.

An example for how to construct a `CustomFilter`:

```dart
CustomFilter createFilter() {
  return CustomFilter.sql(
    where: '${CustomColumns.base.width} > 100 AND ${CustomColumns.base.height} > 200',
    orderBy: [OrderByItem.desc(CustomColumns.base.createDate)],
  );
}
```

#### Advanced CustomFilter

`AdvancedCustomFilter` is a subclass of `CustomFilter`,
it has builder methods to help create the filter.

```dart
CustomFilter createFilter() {
  final group = WhereConditionGroup()
      .and(
        ColumnWhereCondition(
          column: CustomColumns.base.width,
          value: '100',
          operator: '>',
        ),
      )
      .or(
        ColumnWhereCondition(
          column: CustomColumns.base.height,
          value: '200',
          operator: '>',
        ),
      );
  final filter = AdvancedCustomFilter()
      .addWhereCondition(group)
      .addOrderBy(column: CustomColumns.base.createDate, isAsc: false);
  return filter;
}
```

#### Classes explanations

- `CustomFilter`: The base class of custom filter.
- `SqlCustomFilter`: Creates a SQL-like filter.
- `AdvancedCustomFilter`: Creates an advanced filter.
  - `OrderByItem`: The class of ORDER BY item.
  - `WhereConditionItem`: The class of WHERE condition item.
    - `WhereConditionGroup`: Create a group of where condition.
    - `TextWhereCondition`: The text will not be verified if it's valid.
    - `ColumnWhereCondition`: The input column will be verified if it's valid.
    - `DateColumnWhereCondition`: Dates have different conversion methods on iOS/macOS, it helps to smoothest the platform differences.
- `CustomColumns`: This class contains fields for different platforms.
  - `base`: The common fields are included here, but please note that the "id" field is invalid in iOS and may even cause errors. It is only valid on Android.
  - `android`: The columns of the Android platform.
  - `darwin`: The columns of iOS/macOS platforms.

Here is the flow chart to explain how advanced filter works:
![flow_chart](flow_chart/advance_custom_filter.png)

## Cache mechanism

### Cache on Android

Because Android 10 restricts the ability to access the resource path directly,
image caches will be generated during I/O processes.
More specifically, when the `file`, `originFile` and any other I/O getters are called,
the plugin will save a file in the cache folder for further use.

Fortunately, on Android 11 and above, the resource path can be obtained directly again,
but you can still use `requestLegacyExternalStorage`
to access files in the storage without caching them.
See [Android 10 (Q, 29)](#android-10-q-29) for how to add the attribute.
The attribute is not required.

### Cache on iOS

iOS does not directly provide APIs to access the original files of the album.
So a cached file will be generated locally
into the container of the current application
when you called `file`, `originFile` and any other I/O getters.

If occupied disk spaces are sensitive in your use case,
you can delete it after your usage has done (iOS only).

```dart
import 'dart:io';

Future<void> useEntity(AssetEntity entity) async {
  File? file;
  try {
    file = await entity.file;
    await handleFile(file!); // Custom method to handle the obtained file.
  } finally {
    if (Platform.isIOS) {
      file?.deleteSync(); // Delete it once the process has done.
    }
  }
}
```

### Clear caches

You can use the `PhotoManager.clearFileCache()` method
to clear all caches that generated by the plugin.
Here are caches generation on different platforms,
types and resolutions.

| Platform | Thumbnail | File / Origin File |
| -------- | --------- | ------------------ |
| Android  | Yes       | Yes (Android 10+)  |
| iOS      | No        | Yes                |

## Native extra configs

### Android extra configs

#### Glide issues

If your found any conflicting issues against Glide,
then you'll need to edit the `android/build.gradle` file:

```gradle
rootProject.allprojects {
    subprojects {
        project.configurations.all {
            resolutionStrategy.eachDependency { details ->
                if (details.requested.group == 'com.github.bumptech.glide'
                        && details.requested.name.contains('glide')) {
                    details.useVersion '4.15.1'
                }
            }
        }
    }
}
```

See [ProGuard for Glide](https://github.com/bumptech/glide#proguard)
if you want to know more about using ProGuard and Glide together.

#### Android 14 (API 34) extra configs

When targeting Android 14 (API level 34),
the following extra configs needs to be added to the manifest:

```xml
<manifest>
    <uses-permission android:name="android.permission.READ_MEDIA_VISUAL_USER_SELECTED" />  <!-- If you want to use the limited access feature. -->
</manifest>
```

#### Android 13 (API 33) extra configs

When targeting Android 13 (API level 33),
the following extra configs needs to be added to the manifest:

```xml
<manifest>
    <uses-permission android:name="android.permission.READ_MEDIA_IMAGES" /> <!-- If you want to read images-->
    <uses-permission android:name="android.permission.READ_MEDIA_VIDEO" /> <!-- If you want to read videos-->
    <uses-permission android:name="android.permission.READ_MEDIA_AUDIO" /> <!-- If you want to read audio-->
</manifest>
```

### iOS extra configs

#### Localized system albums name

By default, iOS will retrieve system album names only in English
no matter what language has been set to devices.
To change the default language, see the following steps:

- Open your iOS project (Runner.xcworkspace) using Xcode.
  ![Edit localizations in Xcode 1](https://raw.githubusercontent.com/CaiJingLong/some_asset/master/iosFlutterProjectEditinginXcode.png)

- Select the project "Runner" and in the localizations table, click on the + icon.
  ![Edit localizations in Xcode 2](https://raw.githubusercontent.com/CaiJingLong/some_asset/master/iosFlutterAddLocalization.png)

- Select the adequate language(s) you want to retrieve localized strings.
- Validate the popup screen without any modification.
- Rebuild your flutter project.

Now system albums label should display accordingly.

**Note**: Localize is not meant to customize albums name.

### Experimental features

**Warning**: Features here aren't guaranteed to be fully usable
across platforms and OS versions,
since they involved with data modification.
They can be modified/removed in any time,
without following a proper version semantic.

Some APIs will make irreversible modification/deletion to data.
**Please be careful and implement your own test mechanism when using them**.

#### Preload thumbnails

You can preload thumbnails for entities with specified thumbnail options
using `PhotoCachingManager.requestCacheAssets`
or `PhotoCachingManager.requestCacheAssetsWithIds`.

```dart
PhotoCachingManager().requestCacheAssets(assets: assets, option: option);
```

And you can stop in anytime by calling
`PhotoCachingManager().cancelCacheRequest()`.

Usually, when we're previewing assets, thumbnails will be loaded instantly.
But sometimes we want to preload assets to make them display faster.

The `PhotoCachingManager` uses the [PHCachingImageManager][] on iOS,
and Glide's file cache on Android.

#### Delete entities

**This method will delete the asset completely from your device.
Use it with extra cautious.**

```dart
// Deleted IDs will returned, if it fails, the result will be an empty list.
final List<String> result = await PhotoManager.editor.deleteWithIds(
  <String>[entity.id],
);
```

After the deletion, you can call the `refreshPathProperties` method
to refresh the corresponding `AssetPathEntity` in order to get latest fields.

#### Copy an entity

You can use `copyAssetToPath` method to "Copy" an entity
from its current position to the targeting `AssetPathEntity`:

```dart
// Make sure your path entity is accessible.
final AssetPathEntity anotherPathEntity = anotherAccessiblePath;
final AssetEntity entity = yourEntity;
final AssetEntity? newEntity = await PhotoManager.editor.copyAssetToPath(
  asset: entity,
  pathEntity: anotherPathEntity,
); // The result could be null when the path is not accessible.
```

The "Copy" means differently here on Android and iOS:

- For Android, it inserts a copy of the source entity:
  - On platforms <=28, the method will copy most of the origin info.
  - On platforms >=29, some fields cannot be modified during the insertion,
    e.g. [MediaColumns.RELATIVE_PATH][].
- For iOS, it makes a shortcut thing rather than create a new physical entity.
  - Some albums are smart albums, their content is automatically managed
    by the system and cannot insert entities manually.

(For Android 30+, this feature is blocked by system limitations currently.)

#### Features for Android only

##### Move an entity to another album

```dart
// Make sure your path entity is accessible.
final AssetPathEntity pathEntity = accessiblePath;
final AssetEntity entity = yourEntity;
await PhotoManager.editor.android.moveAssetToAnother(
  entity: entity,
  target: pathEntity,
);
```

(For Android 30+, this feature is blocked by system limitations currently.)

##### Remove all non-exist entities

This will remove all items (records) that's not existed locally.
A record in Android `MediaStore` could have the corresponding file deleted.
Those abnormal behaviors usually caused by operations from
file manager, helper tools or adb tool.
This operation is resource-consuming,
Please use the `await` keyword to call the cleaning process
before you call another one.

```dart
await PhotoManager.editor.android.removeAllNoExistsAsset();
```

Some operating systems will prompt confirmation dialogs
for each entities' deletion, we have no way to avoid them.
Make sure you're using the correct method,
and your customers accept repeatedly confirmations.

##### Move entities to trash

```dart
await PhotoManager.editor.android.moveToTrash(list);
```

The method supports Android 11 and above versions.
It will move the entities to trash.
Throws exception when calling on Android 11-.

#### Features for iOS or macOS

##### Create a folder

```dart
PhotoManager.editor.darwin.createFolder(
  name,
  parent: parent, // The value should be null, the root path or other accessible folders.
);
```

##### Create an album

```dart
PhotoManager.editor.darwin.createAlbum(
  name,
  parent: parent, // The value should be null, the root path or other accessible folders.
);
```

##### Remove the entity entry from the album

Remove the entry of the asset from the specific album.
The asset won't be deleted from the device, only removed from the album.

```dart
// Make sure your path entity is accessible.
final AssetPathEntity pathEntity = accessiblePath;
final AssetEntity entity = yourEntity;
final List<AssetEntity> entities = <AssetEntity>[yourEntity, anotherEntity];
// Remove single asset from the album.
// It'll call the list method as the implementation.
await PhotoManager.editor.darwin.removeInAlbum(
  yourEntity,
  accessiblePath,
);
// Remove assets from the album in batches.
await PhotoManager.editor.darwin.removeAssetsInAlbum(
  entities,
  accessiblePath,
);
```

##### Delete `AssetPathEntity`

Smart albums can't be deleted.

```dart
PhotoManager.editor.darwin.deletePath();
```

[pub package]: https://pub.dev/packages/photo_manager
[repo]: https://github.com/fluttercandies/flutter_photo_manager
[GitHub issues]: https://github.com/fluttercandies/flutter_photo_manager/issues
[Glide]: https://bumptech.github.io/glide/
[Generated API]: https://bumptech.github.io/glide/doc/generatedapi.html
[MediaColumns.RELATIVE_PATH]: https://developer.android.com/reference/android/provider/MediaStore.MediaColumns#RELATIVE_PATH
[PHAuthorizationStatus]: https://developer.apple.com/documentation/photokit/phauthorizationstatus?language=objc
[PHCachingImageManager]: https://developer.apple.com/documentation/photokit/phcachingimagemanager?language=objc
[`AssetPathEntity`]: https://pub.dev/documentation/photo_manager/latest/photo_manager/AssetPathEntity-class.html
[`AssetEntity`]: https://pub.dev/documentation/photo_manager/latest/photo_manager/AssetEntity-class.html
[`getAssetPathList`]: https://pub.dev/documentation/photo_manager/latest/photo_manager/PhotoManager/getAssetPathList.html
[`getAssetListPaged`]: https://pub.dev/documentation/photo_manager/latest/photo_manager/AssetPathEntity/getAssetListPaged.html
[`getAssetListRange`]: https://pub.dev/documentation/photo_manager/latest/photo_manager/AssetPathEntity/getAssetListRange.html
[`PhotoManager.getAssetListPaged`]: https://pub.dev/documentation/photo_manager/latest/photo_manager/PhotoManager/getAssetListPaged.html
[`PhotoManager.getAssetListRange`]: https://pub.dev/documentation/photo_manager/latest/photo_manager/PhotoManager/getAssetListRange.html
[`AssetEntity.fromId`]: https://pub.dev/documentation/photo_manager/latest/photo_manager/AssetEntity/fromId.html
[`LocallyAvailableBuilder`]: https://github.com/fluttercandies/flutter_wechat_assets_picker/blob/2055adfa74370339d10e6f09adef72f2130d2380/lib/src/widget/builder/locally_available_builder.dart
[flutter/flutter#20522]: https://github.com/flutter/flutter/issues/20522
[photo_manager_image_provider]: https://pub.dev/packages/photo_manager_image_provider
