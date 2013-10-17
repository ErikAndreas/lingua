lingua
======

Angular i18n library wrapping [Jed](https://github.com/SlexAxton/Jed) which allows for gettext-style support in html and javascript. Supporting singular, plural and interpolation/sprintf. Full stack tooling, like xgettext for text extraction, packaged for use with Grunt at [grunt-lingua](https://github.com/ErikAndreas/grunt-lingua)

## Dependencies
Included in /vendor
* [Jed](https://github.com/SlexAxton/Jed)
* [Microajax](https://code.google.com/p/microajax/)

## Usage
Usage in html/partial/view

```html
<!-- sample usage html/partial/view markup -->
{{_("Your last.fm username")}}:<br/>
<input ng-model="lastFMuserName"/> <button ng-click="setLastFMuserName()">{{_("Set")}}</button>
<h3>{{_n("Suggestion","Suggestions",suggs.length)}}</h3>
```

Usage in service/controller

```js
// sample usage in Angular service
angular.module('modulename').factory('artistAlbumModelService',['linguaService', ... ,function(linguaService, ...) {
  addArtistAlbum:function(ar,al) {
    if (artistAlbumModelService.containsArtistAlbum(ar,al)) {
       statusService.add('error',linguaService._("Skipping duplicate, %1$s %2$s is already in the list",[ar,al]));
  ...
}]);

// sample usage in Angular controller
angular.module('modulename').controller('SettingsCtrl',['$scope', ..., function($scope, ...) {
  statusService.add('info',$scope._("import complete"));
}]);
```

Initialize the module
```js
// Angular init stuff
angular.module('modulename').run(['$rootScope',...,'linguaService',function($rootScope,...,linguaService) {
  $rootScope._ = linguaService._;
  $rootScope._n = linguaService._n;
  ...
}]);
```

And wrap it up on your page
```html
<!doctype html>
<html lang="en" xmlns:ng="http://angularjs.org"> <!-- manual bootstrap so no ng-app -->
...
  <script src="//ajax.googleapis.com/ajax/libs/angularjs/1.1.5/angular.min.js"></script>
  ...
  <script src="js/lingua.js"></script>
  <script src="js/vendor/jed.js"></script>
  <script src="js/vendor/microajax.js"></script>
  <script>
  angular.element(document).ready(function() {
    Lingua.init(document, function() {
      angular.bootstrap(document, ['modulename']);
    });
  });
  </script>
</body>
</html>
```
## Tooling
###Requirements:
1. python + pybabel + jinja2
2. nodejs + >npm install -g po2json

###Simple babel.cfg file:
  [javascript:*.js]
  encoding = utf-8

  [jinja2: *.html]
  encoding = utf-8

###Workflow:
####Generate .pot file:
```shell
pybabel extract -F babel.cfg -k _n:1,2 -k _ -o translations/messages.pot . partials js
```

####Translations
tool like poedit to create a catalog and make translations (outputs .po/.mo files)

####Generate .json
```shell
po2json translations/sv-se.po l_sv-se.json
```

All of the above wrapped up for grunt at [grunt-lingua](https://github.com/ErikAndreas/grunt-lingua)
