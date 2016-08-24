---
title: '[Android] 6.0 系统权限适配实践'
date: 2016-05-28 09:56:06
tags: Android 6.0
category: Android新技术实践
---
>Android 6.0也已经出来有一段时间了，其中的权限模式从一开始的全部列出授予，到后来的运行时动态申请，这对开发者来说是一个重要的变化，今天我来分享一下具体的实践过程。

首先检查一下你的项目中 targetSdkVersion，如果是 23及以上，则必须适配新的权限模式；如果是 23以下，则还是统一在安装时全部申请权限。然后你还需要了解哪些权限是危险权限、特殊权限以及正常权限。

接下来我们需要准备两个类：
PermissionsChecker.java
```
/**
 * 检查权限的工具类
 *
 * @author mafei
 */
public class PermissionsChecker {
  private final Context mContext;

  public PermissionsChecker(Context context) {
    mContext = context.getApplicationContext();
  }

  // 判断权限集合
  public boolean lacksPermissions(String... permissions) {
    for (String permission : permissions) {
      if (lacksPermission(permission)) {
        return true;
      }
    }
    return false;
  }

  // 判断是否缺少权限
  private boolean lacksPermission(String permission) {
    return ContextCompat.checkSelfPermission(mContext, permission)
        == PackageManager.PERMISSION_DENIED;
  }
}
```
PermissionsType.java
```
/**
 * Created by mafei on 16/3/31.
 */
public class PermissionsType {

  /**
   * 读取手机权限
   */
  public static final int READ_PHONE_STATE_CODE = 1;
  /**
   * 获取相机权限
   */
  public static final int CAMERA_CODE = 2;

  /**
   * 获取存储权限
   */
  public static final int WRITE_EXTERNAL_STORAGE_CODE = 3;

  public static class PermissionsTypeExtend {

    public static String toDescription(int type) {
      switch (type) {
        case PermissionsType.READ_PHONE_STATE_CODE:
          return "需要在系统“权限”中打开“电话”开关，才能更好的为你服务";
        case PermissionsType.CAMERA_CODE:
          return "需要在系统“权限”中打开“相机”开关，才能相机拍照";
        case PermissionsType.WRITE_EXTERNAL_STORAGE_CODE:
          return "需要在系统“权限”中打开“存储”开关，才能离线缓存";
        default:
          return "需要在系统“权限”中打开相关权限，才能更好的为你服务";
      }
    }
  }
}
```
准备好这两个类之后，就可以在你需要进行权限申请和控制的地方写下面的代码了：
```
 private PermissionsChecker mPermissionsChecker; // 权限检测器
  // 所需的全部权限
  static final String[] PERMISSIONS = new String[] {
      android.Manifest.permission.WRITE_EXTERNAL_STORAGE
  };

  @Override protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_movie_detail);
    mPermissionsChecker = new PermissionsChecker(this);
  }

  if (VersionSDK.isMarshmallowOrHigher()) {
    String[] permissions = PERMISSIONS;
    if (mPermissionsChecker.lacksPermissions(permissions)) {
       requestPermissions(permissions, PermissionsType.WRITE_EXTERNAL_STORAGE_CODE);
       } else {
         //需要权限才能操作的代码
         }
     } else {
       //需要权限才能操作的代码
    }

  @Override public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions,
      @NonNull int[] grantResults) {
    if (requestCode == PermissionsType.WRITE_EXTERNAL_STORAGE_CODE) {
      for (int i = 0; i < permissions.length; i++) {
        String permission = permissions[i];
        int grantResult = grantResults[i];

        if (permission.equals(android.Manifest.permission.WRITE_EXTERNAL_STORAGE)) {
          if (grantResult == PackageManager.PERMISSION_GRANTED) {
            movieDetailIvCache.setSelected(true);
          } else {
            ToastUtils.showShortToast("需要存储权限");
          }
        }
      }
    }
  }
```
***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
