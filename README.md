# Documentation Addon

This addon allows you to easily add documentation to your widgets by integrating Markdown files located in the `assets/markdown` directory.

## Features

- **Contextual Documentation**: Displays the documentation associated with each of your widgets.
- **Dynamic Loading**: The Markdown content is loaded dynamically based on the widget’s path.
- **Advanced Formatting**: Thanks to `flutter_markdown` and `flutter_highlighter`, you enjoy elegant formatting and syntax highlighting for code blocks.
- **Resizable Layout**: The documentation is displayed in a resizable view, allowing you to compare the widget and its documentation comfortably.

## How It Works

1. **Activating Assets**  
   To allow the addon to load your documentation, ensure that assets are enabled in your Flutter project.

2. **Organizing Markdown Files**  
   Place your Markdown files in the `/assets/markdown/` folder. The filename must correspond to the full widget path (provided via query parameters) followed by the `.md` extension.

   **Example:**  
   If the widget path is `components/button`, the file should be placed as:
```
/assets/markdown/components/button.md
```

3. **Addon Configuration**  
   The addon is enabled via the `documentation` parameter (a boolean value). By default, it is initialized to `true`. You can control its state via query parameters or by configuring it in the Widgetbook addons panel.

## Integration Example

```dart
@widgetbook.App()
class HotReload extends StatelessWidget {
  const HotReload({super.key});

  @override
  Widget build(BuildContext context) {
    Map<String, String> params =
        Map.fromEntries(Uri.base.queryParameters.entries);
    var path = params["path"] ?? 'accueil%2Freadme';
    var knobs = params["knobs"] ?? '{}';
    var device = params["device"] ?? '{name:None}';
    return Widgetbook.material(
      initialRoute: "/?path=$path&knobs=$knobs&device=$device",
      themeMode: ThemeMode.light,
      addons: [
        DocumentationAddon(assetBundle: rootBundle),
        DeviceFrameAddon(devices: [
          ...Devices.android.all,
          ...Devices.ios.all,
          Devices.linux.laptop,
          Devices.windows.wideMonitor,
        ]),
      ],
      directories: [
        WidgetbookComponent(name: "Accueil", useCases: [
          usercaseWithMarkdown(
            "README",
            null,
            "markdown/introduction.md",
          ),
        ]),
        ...directories,
      ],
    );
  }
}

@widgetbook.UseCase(
  name: 'TextDynamicInput.percent',
  type: TextDynamicInput,
  path: 'components/text_dynamic_input',
) // with this configuration, you have to add a markdown file in the assets/markdown/components/text_dynamic_input/textdynamicinput.percent.md
Widget buildPercentTextDynamicInputUseCase(BuildContext context) {
  return Percent();
}
```

## Add automatically your assets on pubspec.yaml

You can use `asset_manager_cli` ([pub.dev link](https://pub.dev/packages/asset_manager_cli))

### Install
```shell
dart pub global activate asset_manager_cli
```

### Using
```shell
asset_manager add
```
