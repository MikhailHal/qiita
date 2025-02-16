<!--
title:   Jetpack ComposeでenableEdgeToEdgeが効かないですよ
tags:    Android,JetpackCompose,Kotlin
id:      b0e335c69d678eec9964
private: false
-->
# enableEdgeToEdgeが効かない
`Kotlin
/**
 * onCreate
 * アクティビティの生成
 * @param savedInstanceState Bundle?
 */
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    enableEdgeToEdge(statusBarStyle = SystemBarStyle.dark(
        Color.Red.toArgb())
    )

    setContent {
        Link22Theme {
            // A surface container using the 'background' color from the theme
            Surface(
                modifier = Modifier.fillMaxSize(),
                color = MaterialTheme.colorScheme.background
            ) {
                Screen()
            }
        }
    }
}
`
と書けば、ステータスバーが赤になるかと思いきやならない...。

# 原因
当該画面のTheme.ktのテーマ設定が悪い。
上記コードだと、「Link22Theme」の部分。
定義元に飛ぶと以下のようなコードがある。

```Kotlin
@Composable
fun Link22Theme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    // Dynamic color is available on Android 12+
    dynamicColor: Boolean = true,
    content: @Composable () -> Unit
) {
    val colorScheme = when {
        dynamicColor && Build.VERSION.SDK_INT >= Build.VERSION_CODES.S -> {
            val context = LocalContext.current
            if (darkTheme) dynamicDarkColorScheme(context) else dynamicLightColorScheme(context)
        }

        darkTheme -> DarkColorScheme
        else -> LightColorScheme
    }
    val view = LocalView.current
    if (!view.isInEditMode) {
        SideEffect {
            val window = (view.context as Activity).window
            window.statusBarColor = colorScheme.primary.toArgb()
            WindowCompat.getInsetsController(window, view).isAppearanceLightStatusBars = darkTheme
        }
    }

    MaterialTheme(
        colorScheme = colorScheme,
        typography = Typography,
        content = content
    )
}
```
この中の下記の部分をコメントアウトすれば適用される。

```Kotlin
val view = LocalView.current
if (!view.isInEditMode) {
    SideEffect {
        val window = (view.context as Activity).window
        window.statusBarColor = colorScheme.primary.toArgb()
        WindowCompat.getInsetsController(window, view).isAppearanceLightStatusBars = darkTheme
    }
}
```

つまり、設定しても上書きされてたってわけですね。
ちょっと沼りました。