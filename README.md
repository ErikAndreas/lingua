lingua
======

Angular i18n library wrapping Jed (gettext for js).

Tooling (xgettext equivalent and .po -> json conversion) at [lingua-tooling](https://github.com/ErikAndreas/linga-tooling)


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
angular.module('swl').factory('artistAlbumModelService',['linguaService', ... ,function(linguaService, ...) {
  addArtistAlbum:function(ar,al) {
    if (artistAlbumModelService.containsArtistAlbum(ar,al)) {
       statusService.add('error',linguaService._("Skipping duplicate, %1$s %2$s is already in the list",[ar,al]));
  ...
}]);

// sample usage in Angular controller
angular.module('swl').controller('SettingsCtrl',['$scope', ..., function($scope, ...) {
  statusService.add('info',$scope._("import complete"));
}]);
```
