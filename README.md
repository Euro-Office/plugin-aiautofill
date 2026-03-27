# AI Auto Fill Plugin for Euro-Office

An Euro-Office plugin that automatically fills document form fields using AI-powered field mapping and third-party data integration.

## Features

- **AI Powered Field Mapping**: Automatically matches form fields with data keys using AI
- **Third-Party Integration**: Connects to external backend services to retrieve data from
- **Code Rotation Security**: Implements secure code rotation mechanism for API calls
- **Flexible Data Handling**: Supports flat, nested, and array data structures
- **User Confirmation**: Review and modify field mappings before applying

## Requirements

- Euro-Office Document Server 9.2.0 or higher
- AI features enabled in Euro-Office
- Backend service implementing the callback API (see [Third-Party Integration Guide](docs/third-party-integration.md))

## Installation

### For End Users

1. Configure the plugin in your Euro-Office Document Server configuration:

```javascript
const config = {
  editorConfig: {
    plugins: {
      autostart: ['asc.{6A95DA5C-857E-4C26-B00B-34876F1EEAD8}'],
      options: {
        'asc.{6A95DA5C-857E-4C26-B00B-34876F1EEAD8}': {
          code: 'your_initial_security_code',
          callback: 'https://your-backend.com/<path_to_data_endpoint>'
        }
      },
      pluginsData: [
        'https://your-backend.com/<path_to_plugin>/config.json'
      ],
      url: 'https://your-backend.com/<path_to_plugin>'
    }
  }
};
```

2. Open a PDF form with form fields in Euro-Office
3. Click the AI Auto Fill plugin icon in the left panel
4. Click "Autofill" to automatically populate form fields

### For Developers

1. Clone this repository
2. Install build tools:
```bash
make install-tools
```

3. Build the plugin:
```bash
make build
```

The built plugin will be in the `build/` directory.

## Usage

### Basic Workflow

1. **Open a Document**: Open a PDF form with form fields in Euro-Office
2. **Launch Plugin**: Click the AI Auto Fill icon in the plugins panel
3. **Start Autofill**: Click the "Autofill" button to begin the process
4. **AI Processing**: The plugin:
   - Detects all form fields in the document
   - Fetches data from your configured backend
   - Uses AI to intelligently map fields to data keys
5. **Review & Confirm**: Review the suggested field mappings and values
6. **Apply**: Click "Apply" to populate the form fields

### Field Mapping Interface

The plugin presents an interactive interface where you can:
- **Select/Deselect Fields**: Choose which fields to fill using checkboxes
- **Modify Values**: Edit or select different values for each field
- **Review Suggestions**: See AI-suggested mappings between form fields and data

## Configuration

See the [Third-Party Integration Guide](docs/third-party-integration.md) for complete API documentation.

## Supported Data Structures

### Flat Data
```json
{
  "firstName": "John",
  "lastName": "Doe"
}
```

### Nested Data
```json
{
  "user": {
    "name": {
      "first": "John",
      "last": "Doe"
    }
  }
}
```
Extracted as: `user.name.first`, `user.name.last`

### Array Data
```json
{
  "employees": [
    {"name": "John", "role": "Manager"},
    {"name": "Jane", "role": "Developer"}
  ]
}
```
Extracted as: `employees.name`, `employees.role`

### How the value matching works

The plugin intelligently matches Pipedrive data with the fields in your Euro-Office PDF form without sending sensitive user data to the AI.

1. **Data request:** The plugin requests available data from Pipedrive. It can retrieve information related to customers, sellers, deals, organizations, and products.
2. **Field analysis:** It then retrieves the key data (or tag) for all fields within the PDF form.
3. **AI-powered matching:** The plugin sends only the data keys and field keys to the AI model. The AI then determines the best matches between Pipedrive data points and your form fields.
4. **Recommendations:** Finally, the plugin displays the AI's recommendations for you to review and apply.

### Usage

**1. Opening a PDF form**

To begin, open a PDF form within Pipedrive from the Euro-Office Documents section.

Please note: It's important that a PDF form must be created in Euro-Office.

**2. Initiating autofill**

When the editor opens, the AI Auto Fill plugin will appear automatically on the side panel.

To proceed with populating the form, click the Autofill button. If you prefer to fill the form manually, simply click Cancel or close the plugin panel.

**3. Reviewing suggestions**

After clicking Autofill, a new window will open, displaying each form field alongside the corresponding value suggested by the AI.

* If the AI identifies multiple potential values for a single field, you can choose the correct one from a drop-down list.
* To skip filling a specific field, uncheck the checkbox next to it.
* To locate a field within the document, click the Open file location button next to it.

**4. Applying the data**

Once you have reviewed the suggestions, click the Apply button. A confirmation prompt will appear, warning you that the form fields will be filled automatically. If you confirm, the selected values will be inserted into the form.

**5. Post-fill options**

After the process completes, the plugin will display a success message.

The Cancel autofill button will become active, allowing you to undo the changes and revert the form to its previous state.

If you have closed the plugin at any point, you can always reopen it from the Plugins tab in the top toolbar of the editor.

