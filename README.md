# Sulu Multi CKEditor Bundle

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PHP Version](https://img.shields.io/badge/php-%5E8.1-blue)](https://php.net/)
[![Sulu](https://img.shields.io/badge/sulu-%5E2.5-green)](https://sulu.io/)

Multiple CKEditor configurations for Sulu CMS - Provides different editor configurations within the same Sulu installation.

## Overview

The Sulu Multi CKEditor Bundle addresses Sulu's limitation of having a single global CKEditor configuration by providing parameter-based editor configurations. This allows you to use different editor toolbars and features for different content fields within the same Sulu admin interface.

## ✨ Features

- **🎯 Multiple Editor Configurations**: Define different editor toolbars per field
- **⚡ Minimal Editor**: Simplified toolbar for basic editing (bold, italic, lists)
- **📝 Simple Editor**: Intermediate toolbar with headings and colors  
- **🚀 Full Editor**: Complete toolbar with all formatting options
- **⚙️ Parameter-based**: Control via XML template parameters
- **🔧 Seamless Integration**: Works with existing Sulu templates
- **♻️ Generic Solution**: Reusable across different Sulu projects
- **📘 TypeScript Support**: Fully typed React components

## 📋 Requirements

- **PHP**: ^8.1
- **Sulu CMS**: ^2.5
- **Symfony**: ^5.4 || ^6.0 || ^7.0
- **Node.js**: ^16 (for asset compilation)

## 📦 Installation

### Step 1: Install via Composer

```bash
composer require stolidbug/sulu-multi-ckeditor-bundle
```

### Step 2: Register the Bundle

Add the bundle to your `config/bundles.php`:

```php
<?php

return [
    // ... other bundles
    Stolidbug\SuluMultiCKEditorBundle\MultiEditorBundle::class => ['all' => true],
];
```

### Step 3: Configure Services

Add to your `config/services.yaml`:

```yaml
# Configurable Text Editor
multi_editor.content_type.configurable_text_editor:
    class: Stolidbug\SuluMultiCKEditorBundle\Content\ConfigurableTextEditor
    arguments:
        - '@sulu_markup.parser'
        - 'sulu'
    tags:
        - { name: sulu.content.type, alias: 'configurable_text_editor' }
```

### Step 4: Copy JavaScript Components

Copy the admin JavaScript files from the bundle to your project:

```bash
# Create directories if they don't exist
mkdir -p assets/admin/{adapters,components,fields}

# Copy the JavaScript components
cp vendor/stolidbug/sulu-multi-ckeditor-bundle/assets/admin/adapters/CKEditor5Configurable.js assets/admin/adapters/
cp vendor/stolidbug/sulu-multi-ckeditor-bundle/assets/admin/components/CKEditor5Configurable.js assets/admin/components/
cp vendor/stolidbug/sulu-multi-ckeditor-bundle/assets/admin/fields/ConfigurableTextEditor.js assets/admin/fields/
```

### Step 5: Update Admin App.js

Add the field type registration to your `assets/admin/app.js`:

```javascript
import {textEditorRegistry, fieldRegistry} from 'sulu-admin-bundle/containers';
import CKEditor5ConfigurableAdapter from './adapters/CKEditor5Configurable';
import ConfigurableTextEditor from './fields/ConfigurableTextEditor';

// Register the adapter and field type
textEditorRegistry.add('ckeditor5_configurable', CKEditor5ConfigurableAdapter);
fieldRegistry.add('configurable_text_editor', ConfigurableTextEditor);
```

### Step 6: Build Assets

```bash
# Build admin assets
npm run build

# Build Sulu admin interface  
bin/adminconsole sulu:build dev
```

### Step 7: Clear Cache

```bash
bin/console cache:clear
```

## 🚀 Usage

### In Templates

Use the `configurable_text_editor` type in your Sulu templates:

```xml
<!-- Minimal editor: bold, italic, lists -->
<property name="intro" type="configurable_text_editor">
    <meta>
        <title lang="en">Introduction</title>
    </meta>
    <params>
        <param name="config" value="minimal"/>
    </params>
</property>

<!-- Simple editor: + headings, underline, colors -->
<property name="content" type="configurable_text_editor">
    <meta>
        <title lang="en">Content</title>
    </meta>
    <params>
        <param name="config" value="simple"/>
    </params>
</property>

<!-- Full editor: all features (default) -->
<property name="description" type="configurable_text_editor">
    <meta>
        <title lang="en">Description</title>
    </meta>
    <!-- No config parameter = full configuration -->
</property>
```

## ⚙️ Available Configurations

| Configuration | Toolbar Features | Use Case |
|---------------|------------------|----------|
| **`minimal`** | Bold, Italic, Lists | Simple text editing, introductions |
| **`simple`** | + Headings (H2-H3), Underline, Colors | Standard content editing |
| **`default`** | + All headings, Alignment, Tables, Code | Rich content, articles |

## 🏗️ Architecture

### Backend Components

- **ConfigurableTextEditor.php**: Content type extending TextEditor
- **MultiEditorBundle.php**: Main bundle class
- **MultiEditorExtension.php**: Dependency injection extension

### Frontend Components

- **ConfigurableTextEditor.js**: Field type component
- **CKEditor5ConfigurableAdapter.js**: Parameter extraction adapter
- **CKEditor5Configurable.js**: CKEditor component with conditional configs

### Data Flow

1. **Template XML** → `config` parameter defined
2. **Sulu Backend** → Parameter passed via `schemaOptions`
3. **Adapter** → Extracts parameter and passes to component
4. **Component** → Selects appropriate configuration
5. **CKEditor** → Instantiated with selected config

## 🧪 Testing

Use the included test script to verify installation:

```bash
# Copy and run the test script
cp vendor/stolidbug/sulu-multi-ckeditor-bundle/test_multi_editor.sh .
chmod +x test_multi_editor.sh
./test_multi_editor.sh
```

This verifies:
- ✅ Admin interface accessibility
- ✅ Content type registration
- ✅ Asset compilation
- ✅ Template structure

## 🔧 Extending

### Adding New Configurations

1. Edit `assets/admin/components/CKEditor5Configurable.js`
2. Add new condition in `getConfigForType()` method:

```javascript
if (configType === 'my_custom_config') {
    return {
        ...baseConfig,
        toolbar: ['bold', 'italic', 'heading', 'bulletedList'],
        heading: {
            options: [
                { model: 'paragraph', title: 'Paragraph', class: 'ck-heading_paragraph' },
                { model: 'heading2', view: 'h2', title: 'Heading 2', class: 'ck-heading_heading2' }
            ]
        }
    };
}
```

3. Rebuild assets: `npm run build`

### Custom Configuration Example

```xml
<property name="custom_field" type="configurable_text_editor">
    <meta>
        <title lang="en">Custom Field</title>
    </meta>
    <params>
        <param name="config" value="my_custom_config"/>
    </params>
</property>
```

## 🐛 Troubleshooting

### Build Errors
```bash
# Clear everything and rebuild
bin/console cache:clear
bin/adminconsole sulu:build dev  
npm run build
```

### Field Not Showing
1. ✅ Bundle registered in `config/bundles.php`
2. ✅ Services configured in `config/services.yaml`
3. ✅ JavaScript components registered in `app.js`

### Same Configuration for All Editors
1. Check parameter extraction in adapter
2. Verify conditional logic in component
3. Ensure parameters correctly defined in template

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👤 Author

**Christophe Lignères**
- GitHub: [@stolidbug](https://github.com/stolidbug)
- Email: christophe@akawaka.fr

## 🤝 Contributing

Contributions, issues and feature requests are welcome!

Feel free to check the [issues page](https://github.com/stolidbug/sulu-multi-ckeditor-bundle/issues).

## 🌟 Show your support

Give a ⭐️ if this project helped you!

## 🗺️ Roadmap

- [ ] Visual configuration builder
- [ ] More preset configurations  
- [ ] Plugin system for custom toolbars
- [ ] Template-level configuration inheritance
- [ ] Configuration validation and documentation
