# react-native-twig
Twig templating for use in React Native for print templates

```
  import Twig from '@rokebrand/react-native-twig';

  const properties = {
    title: 'hello there',
  }

  const rawTemplate =
    '<div style="border-bottom:1px solid;">\n' +
    '<div style="font-size: 9px; color: rgb(255, 0, 0); text-align: center; font-family:Arial,Helvetica,sans-serif;"><strong>Template</strong><br />\n' +
    '{{title}}</div>\n' +
    '</div>';

  const template = Twig.twig({
    data: rawTemplate,
  });

  console.log(template.render(properties))
  
```
