---
title: "[ Flutter ] 플러터를 이용한 윈도우용 앱 개발 (2)" 
sub: post
author: Kwangsoo Seo
date: 2022-12-09 06:00:00 +0900
categories: [프로그래밍, Flutter]
tags: [Flutter 3.0, 플러터 3.0, Windows]
sitemap:
  changefreq: weekly
  priority : 0.5
---
[![HitCount](https://hits.dwyl.com/MonosLab/post19.svg?style=flat-square&show=unique)](http://hits.dwyl.com/MonosLab/post19)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---  
# Windows용 Flutter 프로젝트 생성   
```   
> mkdir Sample   
> cd Sample   
> flutter create --template=app --platforms=windows nfbrowser   
  Creating project nfbrowser...   
  Running "flutter pub get" in nfbrowser...                        1,506ms   
  Wrote 27 files.   

  All done!   
```   
---

# 프로젝트 불러오기   
* Android studio 실행   
* 생성된 프로젝트 불러 오기   
** 상단 메뉴 > File > Open을 차례로 눌러 조금전에 생성한 디렉토리로 이동하여 프로젝트를 오픈합니다.   
** shift + F10 또는 메뉴의 run > Run 'main.dart' 를 클릭하여 컴파일 후 정상적으로 윈도우가 뜨는지 확인합니다.   

---

# 브라우저[^footnote_1] 만들기   

## 패키지 설치   
pubspect.yaml 파일의 프로젝트에서 사용할 dependencies에 패키지를 추가합니다.   

```
dev_dependencies:
  webview_windows: ^0.2.2
  system_tray: ^2.0.2
  window_manager: ^0.2.8
  easy_localization: ^3.0.0
  window_size:
    git:
      url: https://github.com/google/flutter-desktop-embedding.git
      path: plugins/window_size
      ref: 5c51870ced62a00e809ba4b81a846a052d241c9f
```
순서대로   
* webview_windows 패키지는 웹브라우저를 개발하기 위한 패키지입니다.   
* system_tray는 윈도우의 시스템 트레이 영역에 아이콘을 표시를 위해 사용합니다.   
* window_manager는 시스템 종료 버튼에 다른 액션을 주기 위해 사용하였습니다.(종료 버튼 클릭시 숨김 처리)  
* easy_localization은 다국어를 위한 패키지입니다.   
* window_size는 아직 공홈에서 지원하는 패키지가 아니어서 git에서 직접 받아와서 사용하였습니다. 윈도우의 타이틀 및 사이즈 변경을 위하여 사용하였습니다.    

차례대로 main.dart 파일에 모두 import 시켜줍니다.   
```
import 'package:webview_windows/webview_windows.dart';
import 'package:system_tray/system_tray.dart';
import 'package:window_manager/window_manager.dart';
import 'package:easy_localization/easy_localization.dart';
import 'package:nfbrowser/model/nfbrowser.dart';
import 'package:window_size/window_size.dart'; 
```   
* webview_windows : 웹브라우저 위젯   
* system_tray : 시스템 트레이 위젯   
* window_manager : 윈도우 종료 관련 처리를 위한 위젯   
* easy_localization : 다국어 처리를 위한 위젯   
* window_size : 윈도우 타이틀, 사이즈 변경을 위한 위젯   

---   

메인에서 다국어 처리를 하고 MyApp 위젯을 호출합니다.

```
void main() async {
  // easylocalization 초기화
  await EasyLocalization.ensureInitialized();

  runApp(EasyLocalization(
      supportedLocales: [ const Locale('en', 'US'), const Locale('ko', 'KR')],
      path: 'assets/lang',                            // 언어 파일 경로
      fallbackLocale: const Locale('en', 'US'),   // fallbackLocale supportedLocales에 설정한 언어가 없는 경우 설정되는 언어
      child: const MyApp()));
}
```

---

```
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        debugShowCheckedModeBanner: false,  // 디버그 표시를 없앰.
        navigatorKey: NfBrowser.naviagatorState,
        // 기본적으로 필요한 언어 설정
        localizationsDelegates: context.localizationDelegates,
        supportedLocales: context.supportedLocales,
        locale: context.locale,
        home: const NfBrowser());
  }
}
```

MyApp 위젯에서 앱의 상단의 디버그 표시를 없애고 홈으로 NfBrowser()를 설정해 줍니다.

```
class NfBrowser extends StatefulWidget {
  const NfBrowser({Key? key}) : super(key: key);

  static final GlobalKey<NavigatorState> naviagatorState = GlobalKey<NavigatorState>();

  @override
  _NfBrowserState createState() => _NfBrowserState();
}

class _NfBrowserState extends State<NfBrowser> with WindowListener {
  final _appWindow = AppWindow();
  final _systemTray = SystemTray();
  final _menuMain = Menu();
  final _webview = WebviewController();
  final _textEdit = TextEditingController();
  final _isWindows = Platform.isWindows;

  late NFBrowser nfbrowser;

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,  // 디버그 표시를 없앰.
      navigatorKey: navigatorKey,
      home: Scaffold(
        body: Center(
          child: compositeView(),
        ),
      ),
    );
  }

  Widget compositeView() {
    if (!_webview.value.isInitialized) {
      return const Text(
        'Not Initialized',
        style: TextStyle(
          fontFamily: '맑은 고딕',
          fontSize: 24.0,
          fontWeight: FontWeight.w700, //.w900,
        ),
      );
    } else {
      return Container(
        // padding: EdgeInsets.all(10),
        padding: EdgeInsets.zero,
        child: Column(
          children: [
            Expanded(
                child: Card(
                    color: Colors.transparent,
                    elevation: 0,
                    clipBehavior: Clip.antiAliasWithSaveLayer,
                    shape: RoundedRectangleBorder(
                      borderRadius: const BorderRadius.only(    // if you need this
                        topLeft: Radius.circular(25),
                        topRight: Radius.circular(25),
                      ),
                      side: BorderSide(
                        color: Colors.grey.withOpacity(0.2),
                        width: 1,
                      ),
                    ),
                    child: Stack(
                      children: [
                        Webview(
                          _webview,
                          permissionRequested: (url, permissionKind, isUserInitiated) => WebviewPermissionDecision.allow,
                        ),
                        StreamBuilder<LoadingState>(
                            stream: _webview.loadingState,
                            builder: (context, snapshot) {
                              if (snapshot.hasData && snapshot.data == LoadingState.loading) {
                                return const LinearProgressIndicator();
                              } else {
                                return Container();
                              }
                            }
                        ),
                      ],
                    )
                )
            ),
          ],
        ),
      );
    }
  }

  @override
  void initState() {
    windowManager.addListener(this);
    super.initState();

    loadInit();
  }

  void loadInit() async {
    await loadData();
    await init();
    await initSystemTray();
    await initPlatformState();

    if (Platform.isWindows || Platform.isLinux || Platform.isMacOS) {
      setWindowTitle('appname'.tr());
      setWindowMaxSize(Size.infinite);
    }
  }

  @override
  void dispose() {
    windowManager.removeListener(this);
    super.dispose();
  }

  @override
  void onWindowEvent(String eventName) {
    print('[WindowManager] onWindowEvent: $eventName');
  }

  @override
  void onWindowClose() async {
    // do something
    bool isPreventClose = await windowManager.isPreventClose();
    if (isPreventClose) {
      _appWindow.hide();
    }
  }

  @override
  void onWindowFocus() {
    // Make sure to call once.
    setState(() {});
    // do something
  }

  Future<bool> loadData() async {
    String data = await rootBundle.loadString("assets/NFBrowser.json");
    final jsonResponse = json.decode(data);
    nfbrowser = NFBrowser.fromJson(jsonResponse);
    debugPrint('= [JSON] ==============================================');
    debugPrint(jsonResponse.toString());
    debugPrint('=======================================================');
    setState(() {});
    return true;
  }

  Future<void> init() async {
    // Add this line to override the default close handler
    await windowManager.setPreventClose(true);
    setState(() {});
  }

  Future<void> initSystemTray() async {
    List<String> iconList = ['open', 'exit'];

    // We first init the systray menu and then add the menu entries
    await _systemTray.initSystemTray(iconPath: getTrayImagePath('app_icon'));
    _systemTray.setTitle('appname'.tr());
    _systemTray.setToolTip('appver'.tr(namedArgs: {'name': 'appname'.tr(), 'ver': 'Ver. 1.0.1'}));
    _systemTray.setContextMenu(_menuMain);
    // handle system tray event
    _systemTray.registerSystemTrayEventHandler((eventName) {
      debugPrint("eventName : $eventName");
      if(eventName == kSystemTrayEventClick) {
        _isWindows ? _appWindow.show() : _systemTray.popUpContextMenu();
      } else if (eventName == kSystemTrayEventRightClick) {
        _isWindows ? _systemTray.popUpContextMenu() : _appWindow.show();
      }
    });

    await _menuMain.buildFrom(
      [
        MenuItemLabel(label: 'open'.tr(),
          image: getImagePath('open'),
          onClicked: (menuItem) {
            debugPrint("Open Menu");
            _appWindow.show();
          }
        ),
        MenuSeparator(),
        MenuItemLabel(label: 'exit'.tr(),
          image: getImagePath('exit'),
          onClicked: (menuItem) {
            debugPrint("Exit Menu");
            //_appWindow.close();
            windowManager.destroy();
          }
        ),
      ]
    );
  }

  Future<void> initPlatformState() async {
    var adminauto = nfbrowser.adminauto;
    var protocol = nfbrowser.protocol;
    var domain = nfbrowser.domain;
    debugPrint('>> adminauto : ${adminauto.toString()}');
    debugPrint('>> protocol : ${protocol.toString()}');
    debugPrint('>> domain : $domain');

    try {
      await _webview.initialize();

      _webview.url.listen((url) {
        _textEdit.text = url;
      });

      late String url;
      if(protocol == 1) {
        url = 'https://$domain';
      }
      else {
        url = 'http://$domain';
      }

      await _webview.setBackgroundColor(Colors.transparent);
      await _webview.setPopupWindowPolicy(WebviewPopupWindowPolicy.allow);
      await _webview.loadUrl(url);

      if (!mounted) return;

      setState(() {});

      _appWindow.show();
    } on PlatformException catch(e) {
      debugPrint(e.toString());
    }
  }

  Future<WebviewPermissionDecision> _onPermissionRequested(
      String url, WebviewPermissionKind kind, bool isUserInitiated) async {
    final decision = await showDialog<WebviewPermissionDecision>(
      context: navigatorKey.currentContext!,
      builder: (BuildContext context) => AlertDialog(
        title: const Text('WebView permission requested'),
        content: Text('WebView has requested permission \'$kind\''),
        actions: <Widget>[
          TextButton(
            onPressed: () =>
                Navigator.pop(context, WebviewPermissionDecision.deny),
            child: const Text('Deny'),
          ),
          TextButton(
            onPressed: () =>
                Navigator.pop(context, WebviewPermissionDecision.allow),
            child: const Text('Allow'),
          ),
        ],
      ),
    );

    return decision ?? WebviewPermissionDecision.none;    // 왼쪽 표현식 값이 null이 아니면 왼쪽 값을 null이면 오른쪽 값을 리턴한다.
  }
}
```

---

# 개발하면서 느낀점

* 패키지   
  * 패키지에 원하는 기능이 "올인원"인 경우는 드물어서 다수개의 패키지를 설치하여 사용하여야 합니다.   
  * 패키지의 기능 수정을 위해서는 C/C++을 알아야 하며, plugin 개발 방법도 함께 숙지하여야 합니다.   
  * 간단한 프로그램을 만들더라도 다수개의 패키지를 설치/관리하여야 하는 상황이 발생합니다.   

* 디바이스 지원   
  * 패키지가 모바일/데스크탑 모두 지원하는 경우도 있지만 그렇지 않은 경우도 있기 때문에 최악의 경우 모바일/윈도우/맥OS/리눅스의 패키지를 찾아서 따로 설치하여야 하는 경우 발생할 수도 있습니다.(이 경우는 디바이스에 의존적인 자원을 사용하는 경우 각 시스템별 사용법이 달라서 패키지 개발을 따로 특정 디바이스에 맞춰 개발한 경우에 발생함)   
  * 윈도우의 경우 아직 지원이 많이 부족하여, plugin을 만들어 사용해야하는 경우가 발생할 수 있습니다. (plugin의 기술력이 요구됨)   

* DPI awareness   
  * 윈도우의 멀티모니터 지원시 각각의 배율을 설정하여 사용하는데, 이를 해지할 수 있는 패키지가 없습니다.   
  * DPI 관련해서는 플루터 엔진에서 설정된 내용을 가져올 수만 있고 셋팅하는 부분은 따로 존재하지 않습니다.   

* Plugin 개발   
  * Windows에서 Flutter 플러그인을 개발하려면 주로 C++ 및 MFC API를 사용하여 Flutter 코덱을 통해 네이티브 플러그인과 Flutter 간에 통신합니다.   
  * 또 다른 방법은 FFI를 사용하여 Dart에서 많은 win32 API를 캡슐화하고 Dart를 직접 사용하여 win32 앱을 작성 하는 Dart 패키지 win32 와 같이 Dart가 FFI 를 통해 Windows API를 호출 하도록 하는 것입니다.   

---   

[^footnote_1]: 브라우저 : 윈도우의 WebView2를 이용한 브라우저.