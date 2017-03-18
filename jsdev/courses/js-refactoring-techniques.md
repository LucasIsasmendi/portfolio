# JS Refactoring Techniques
### The Techniques
1.  Construct HTML using template strings
2.  Eliminate if/else blocks with hash maps
3.  Collapse multiple arguments with a config object
4.  Pre-bind arguments to make point-free functions
5.  Break complex conditionals with intention revealing variables
6.  Inline IIFE for complex initialization
7.  Use hoisting/composed-method to focus on important code

#### 1.  Construct HTML using template strings

``` js
var title = 'My Card',
  photoUrl = 'http://placehold.it/350x150';

var html = `<div class="mdl-card">
    <div class="mdl-card__title">${title}</div>
    <div class="mdl-card__media">
      <img src="${photoUrl}" alt="">
    </div>
  </div>`
```

#### 2.  Eliminate if/else blocks with hash maps
``` js
function getWeatherCondition(condition) {
  var conditionMap = {
    sunny: '/images/sunny.png',
    rainy: '/images/rainy.png',
    snow: '/images/snow.png'
  };
  return conditionMap[condition] || '/images/error.png';
}
```
if we have a large number of conditions, move them to a file and import it.
``` js
import conditionMap from './conditions';
function getWeatherCondition(condition) {
  return conditionMap[condition] || '/images/error.png';
}
```

#### 3.  Collapse multiple arguments with a config object
``` js
function formatNumber({value=0, asPercent=false, prefic='', sufix=''}){
  var formattedNumber = value;
  // some logic
  return formattedNumber
}
var num = 20;
formatNumber({value: num, suffix:'%'});
```

#### 4.  Pre-bind arguments to make point-free functions
``` js
function doOperation(name='default', args={}){
  //Use the args to do operation by name
}
function findSport() {
  //some complex Work
  return Promise.resolve('TableTennis');
}
function getShoppingList() {
  //some complex Work
  return Promise.resolve({milk:2, eggs:12, orange:6});
}
findSport().then(args=> {
  doOperation('playSport', args);
});
getShoppingList().then(args=> {
  doOperation('buyGroceries', args);
});
var play = doOperation.bind(null, 'playSport');
var buy = doOperation.bind(null, 'buyGroceries');
findSport().then(play);
getShoppingList().then(buy);
```

#### 5.  Break complex conditionals with intention revealing variables
In this way, if we add more conditions in the future, it will be easy in the conditions[]
``` js
import _ from 'lodash';
var age, gamer={}, adNetworks=[], specialAchievements=[];
function showSpecialOffer(){
  // show a banner with the special offer
}  
//before
var isTeenager = (age > 0 && age < 20),
  isFirstTimeGamer = gamer.isFirstTime,
  isPartOfAdNetwork = _.some(adNetworks, function(n){
    return gamer.network === n;
  }),
  hasSpecialAchievement = _.some(specialAchievements, function(ach){
    return gamer.lastAchievement == ach;
  });
if (isTeenager || isFirstTimeGamer || isPartOfAdNetwork ||
   hasSpecialAchievement){
     showSpecialOffer();
}
//new way
var conditions = [
  ()=>{ return age > 0 && age < 20},
  ()=> gamer.isFirstTime,
  ()=> {
    return _.some(adNetworks, (n)=>gamer.network === n)
  },
  ()=> {
    return _.some(specialAchievements, (ach)=>gamer.lastAchievement === ach)
  }
];
let matches = _.some(conditions, c=>c());
if(matches){
  showSpecialOffer();  
}
```

#### 6.  Inline IIFE for complex initialization
``` js
var defaultConfig = (()=> {
  var settings = readSettings();
  var apiUrl = `https://${settings.apiDomain}:${settings.apiPort}/api`;
  return {
    apiUrl: apiUrl,
    timezone: settings.timezone,
    account: settings.account,
    theme: settings.theme
  };
  function readSettings() {
    // read from local Storage
    return {/* ... */};
  }
})();
```

#### 7.  Use hoisting/composed-method to focus on important code
hoisting moves the variables and function declarations to the top of the code. We can use this behavior to make the code more redeable.
To hide the implementation detail below the main entry point
``` js
let View = {
  render(){
    var url = `http:localhost:3000/api/${this.accountType}/${this.accountId}`;
    this._fetchData(url)
      .then(processData)
      .then(renderHtml);
  },
  _fetchData(url){
    return Promise.resolve([/*some data*/]);
  }
};

function processData(data) {
  var previousTransactions = data
    .filter(x => {
      return x.timestamp < Date.now();
    })
    .map(x => {
      return {
        type: x.transactionType,
        amount: x.transactionAmount,
        timestamp: x.timestamp
      }
    });
  return previousTransactions;
}
function renderHtml(transactions) {
  var rows = transactions.map(x=> {
    return `<tr>
      <td>${x.type}</td>
      <td>${}x.amount</td>
    </tr>`;
  });
  var html = `<table> ${rows.join('')}</table>`;
  $('#container').html(html);
}
```
