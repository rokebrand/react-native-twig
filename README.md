# react-native-twig

Twig templating for use in React Native for generating HTML from twig templates

Use together with the following packages to create dynamic PDF documents generated from html twig templates for printing and sharing.

- react-native-html-to-pdf
- react-native-print
- react-native-share
- react-native-fs

## React-native-twig Usage

### Import react-native-twig into your file

```

  import Twig from '@rokebrand/react-native-twig';

```

### An example js object that will get passed to the template builder along with the template

```

  const printTitle = 'React-native TWIG Template'

  const properties = {
    pageHeading: printTitle,
    date: '30 March 2023',
    time: '15:30',
    items: [
      {
        id: 1,
        name: 'Test Product 1',
        price: '$10.00',
        quantity: 100
      },
      {
        id: 2,
        name: 'Test Product 2',
        price: '$20.00',
        quantity: 200
      },
      {
        id: 3,
        name: 'Test Product 3',
        price: '$30.00',
        quantity: 300
      }
    ]
  }

```

### An example of a TWIG template that will generate HTML

```

const rawTemplate = `
  <div style="padding: 50px;">
    <h1>{{pageHeading}}</h1>
    <table>
      <tbody>
        <tr>
          <td><strong>Date:</strong></td>
          <td>{{date}}</td>
        </tr>
        <tr>
          <td>
            <span><strong>Time:</strong></span>
          </td>
          <td>{{time}}</td>
        </tr>
      </tbody>
    </table>
    <table>
      <tr>
        <th>Product Id</th>
        <th>Product Name</th>
        <th>Price</th>
        <th>Quantity</th>
      </tr>
      {% for item in items %}
      <tr>
        <td>{{item.id}}</td>
        <td>{{item.name}}</td>
        <td>{{item.price}}</td>
        <td>{{item.quantity}}</td>
      </tr>
      {% endfor %}
    </table>
  </div>`;

```

### Generating the compiled HTML

```

  const template = Twig.twig({
    data: rawTemplate,
  });

  const compiled = template.render(properties);

  // This will log out the compiled HTML
  console.log(compiled)

```

### Generating and printing the PDF

```

  // Prepare print options
  const options = {
    html: compiled,
    fileName: printTitle.replace(' ', '-').toLowerCase(),
    directory: 'Documents',
    width: 210,
    height: 297,
    bgColor: 'white',
  };

  // Convert compiled HTML to PDF
  RNHTMLtoPDF.convert(options).then((value: {filePath: string}) => {
    const pdfPath = Platform.OS === 'android' ? `file://${value.filePath}` : `${value.filePath}`;

```

### Print the PDF with react-native-print

```

  RNPrint.print({ filePath: pdfPath });

```

### Share the PDF with react-native-share

```

  Share.open({
    title: printTitle,
    message: 'custom message',
    url: pdfPath,
  })

```
