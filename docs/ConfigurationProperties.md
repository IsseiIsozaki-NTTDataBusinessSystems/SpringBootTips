# Configuration Properties(プロパティの構成とローディング)
## 1. `ConfigurationProperties` の有効化
下記のようにConfigクラスやmainクラスでアノテーションを指定する。

- `@EnableConfigurationProperties` を利用する方法
  - 下記のようにプロパティを設定するクラスを指定してアノテーションを定義
   ```java
   @Configuration
   @EnableConfigurationProperties({HogeProperties.class})
   ...omit
   ```
- `@ConfigurationPropertiesScan` を利用する方法
  - 下記のようにプロパティを設定するクラスの格納パッケージを指定してアノテーションを定義
   ```java
   @Configuration
   @ConfigurationPropertiesScan({"test.example.hogehoge.props"})
   ...omit
   ```

## 2. `ConfigurationProperties` の実装クラス作成
`ConfigurationProperties` を利用してプロパティをマッピングするクラスを作成。<br>
SpringBoot3.xから `@ConstructorBinding` を利用しなくてもバインディングできるようになっている。

```java
@Getter
@RequiredArgsConstructor
@ConfigurationProperties(prefix = "hogehoge")
public HogeProperties {
    private final String prop1;
    private final String prop2;
}
```

## 3. `application.yaml` でプロパティ値定義
`.properties` でも良いがお好みで。ここまでやればPropertiesクラスをDIして利用可能。
```yaml
hogehoge:
  prop1: testString1
  prop2: testString2
```

## 4. 注意点
- `ConfigurationProperties` を指定するクラスは `@Component` などの指定は不要。
  - `ConfigurationProperties` としてBean登録される。
- `Configuration` クラスとコンストラクタバインディングの共存は今のところ実施できていない。
  - `Configuration` クラスにプロパティ値をバインドしたい場合は `@Setter` でセッターインジェクション方式にすれば解決は可能。
  - `ConfigurationProperties` の同時指定も忘れずに。