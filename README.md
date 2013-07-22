lingua
======

Angular i18n library wrapping Jed which allows for gettext-style support in markup (html and javascript) supporting singular, plural and interpolation/sprintf.

## Tooling
xgettext equivalent and .po -> json conversion) at [lingua-tooling](https://github.com/ErikAndreas/lingua-tooling)

## Dependencies
Included in /vendor
* [Jed](https://github.com/SlexAxton/Jed)
* [Microajax](https://code.google.com/p/microajax/)

## Usage
Usage in html/partial/view

```
<!-- sample usage html/partial/view markup -->
{{_("Your last.fm username")}}:<br/>
<input ng-model="lastFMuserName"/> <button ng-click="setLastFMuserName()">{{_("Set")}}</button>
<h3>{{_n("Suggestion","Suggestions",suggs.length)}}</h3>
```

Usage in service/controller

```
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
```
// Angular init stuff
angular.module('modulename').run(['$rootScope',...,'linguaService',function($rootScope,...,linguaService) {
  $rootScope._ = linguaService._;
  $rootScope._n = linguaService._n;
  ...
}]);
```

And wrap it up on your page
```
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