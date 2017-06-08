# Checklist JS review

## 1. Comments

#### 1.1 Are the comments written useful ? 


````javascript
/**
 * BAD
 */

/**
 * Destroy the module
 */
function destroy() {
  showSubMenus();
  removeIconMinusHeadings();
  shutdownEventCollapse();
  initialized = false;
}
````

````javascript
/**
 * GOOD
 */

/**
 * Go back to clean state (before the module was injected)
 */
function destroy() {
  showSubMenus();
  removeIconMinusHeadings();
  shutdownEventCollapse();
  initialized = false;
}
````

#### 1.2 Does the module contain a global description comment?

````javascript
/**
 * BAD
 * Browse Actions module
 */
WEBLINC.registerModule('browseActions', (function () {}));
````

````javascript
/**
 * GOOD
 * On browse products section (viewport mobile), this module
 * is in charge to open the drawer's filters.
 */
WEBLINC.registerModule('browseActions', (function () {}));
````


### 2. Variables

#### 2.1 Descriptive Variables

````javascript
/**
 * BAD
 */
var prodNb = 0;
````


````javascript
/**
 * GOOD
 */
var countProducts = 0;
````


#### 2.2 No business logic in the variables

````javascript
/**
 * BAD
 */
var $products = $scope.is('.pagination-results') ? $('.product-grid__cell', $scope) : $('.product-grid__cell', $('.view', $scope)),
````

````javascript
/**
 * GOOD
 */
var $products = getProducts();
````


### 3. Conditions

#### 3.1 Does each complex conditions are abstracted ?

````javascript
/**
 * BAD
 */
function showFunnyPictureOnMobile() {
  
  // If it's on mobile
  if (Modernizr.touch && $(window).width() < 768) {
    $('#funny-picture').attr('src', 'funny-picture.jpg');
  }

}
````

````javascript
/**
 * GOOD
 */
function showFunnyPictureOnMobile() {
  
  if (isMobile()) {
    $('#funny-picture').attr('src', 'funny-picture.jpg')
  }

}

function isMobile() {
  return Modernizr.touch && $(window).width() < 768;
}
````

#### 3.2 Does each conditions are defensive?

````javascript
/**
 * BAD
 */
function isDead(zombie) {

  if (!zombie.dead) {

    if (zombie.health <= 0) {
      return true;
    } else {
      return false;
    }

  } else {
    return false;
  }

}
````

````javascript
/**
 * GOOD
 */
function isDead(zombie) {
  
  if (zombie.dead) {
    return true;
  }

  if (zombie.health <= 0) {
    return true;
  }

  return false;

}
````


### 4. jQuery selectors

#### 4.1 Does each jQuery selectors are abstracted?


````javascript
/**
 * BAD
 */
function init() {
  $('.title', $scope).find('span a').remove();
}
````

````javascript
/**
 * GOOD
 */
function init() {
  getTitleLinks().remove();
}

function getTitleLinks() {
  return $('.title', $scope).find('span a');
}
````

#### 4.2 Does each jQuery selectors are scoped?


````javascript
/**
 * BAD
 */
function init() {
  $('.title', $scope).find('span a').remove();
}
````

````javascript
/**
 * GOOD
 */
function init() {
  getTitleLinks().remove();
}

function getTitleLinks() {
  return $('.title', $scope).find('span a');
}
````

## 5. Functions

#### 5.1 Does each function do one thing and only one thing and doesn't contain more than ~15 lines ?

````javascript
/**
 * BAD
 * This function does too much
 */
function handleCheckboxChange(event) {
    productId = $(this).val();
    $compareFormInput = $('input[value="' + productId + '"]', $('form', $productCompare));
    tempData = $compareFormInput.data();

    // if checking this checkbox
    if (checkboxIsChecked(event.currentTarget)) {
        // first, check if the max number of items is checked
        if (numberOfChecked(event) >= MAX_PRODUCTS) {
            window.alert(WEBLINC.config.compare.max_products_message);
            $(event.currentTarget).prop('checked', false);
        } else {
            // check the corresponding one in the compare form
            $compareFormInput.prop('checked', true);

            // add modifier to product summary element.  Allows style changes on products selected for comparison .
            toggleProductSummaryModifier($(event.currentTarget));

            // add to compare display
            addProductDisplay(productId, tempData.imageUrl, tempData.prodName, $productCompare);

            // if more than one checkbox is checked, display compare link
            if (numberOfChecked(event) > 1) {
                updateCheckedLabel($checkboxes.filter(':checked'));
                enableCompareButton($productCompare);
            }
        }
    } else {
        // uncheck the corresponding one in the compare form
        $compareFormInput.prop('checked', false);

        // always change the label of this checkbox
        updateUncheckedLabel($(event.currentTarget));

        // remove modifier from product summary element.
        toggleProductSummaryModifier($(event.currentTarget));

        // remove from compare display
        removeProductDisplay(productId, $productCompare);

        // and if less than two checkboxes total are checked, hide the compare link for all
        if (numberOfChecked(event) < 2) {
            updateUncheckedLabel($checkboxes);
            disableCompareButton($productCompare);
        }
    }

    // update bottom compare display
    updateBottomCompare($productCompare, $productCompare.closest('.view'));

    // on checking/unchecking checkbox, set cookie
    _.compose(setCookie, getCheckedProductIds)($productCompare);
};

````

``` javascript
/**
 * GOOD
 */
function setupCollapse() {
  hideSubMenus();
  registerEventCollapse();
}
```

#### 5.2 Does each anonymous functions are abstracted ?

````javascript
/**
 * BAD
 */
function getAdminUsers(users) {
  
  var admins = _.filter(users, function(user) {
    
    return user.role === 'admin';
    
  });

  return admins;
}
````

````javascript
/**
 * GOOD
 */
function getAdminUsers(users) {
  
  var admins = _.filter(users, isAdmin);

  return admins;

}

function isAdmin(user) {
  return user.role === 'admin';
}
````

````javascript
/**
 * BAD
 */
function init() {

  $('.chocobo').on('click', function(e) {
    var $chocobo = $(e.currentTarget);

    if ($chocobo.data('can-ride') === 'fuck-yeah-you-can') {
      rideChocobo($chocobo);
    } 

  });

}
````

````javascript
/**
 * GOOD
 */
function init() {
  $('.chocobo').on('click', onClickChocobo);
}

function onClickChocobo(e) {
  var $chocobo = $(e.currentTarget);

  if ($chocobo.data('can-ride') === 'fuck-yeah-you-can') {
    rideChocobo($chocobo);
  } 

}
````

#### 5.3 Does each function name are descriptive enough ?

````javascript
/**
 * BAD
 */
function noProduct($productId) {}
````

````javascript
/**
 * GOOD
 */
function isProductExists($productId) {}
````

#### 5.4 Does each function doesn't have more than 3 arguments ?


````javascript
/**
 * BAD
 */
function killUser(id, name, level, email, birthday) {}
````

````javascript
/**
 * GOOD
 */

function killUser(id, attributes) {}

/**
 * Where attributes is an object
 * {name: "peter", level: 20, email: "peter@gmail.com", birthday: null}
 */
````
