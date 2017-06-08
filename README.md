# Checklist JS review


## 1. Functions

#### 1.1 Does each function do one thing and only one thing ?

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

#### 1.2 Does each anonymous functions are abstracted ?


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

#### 1.3 Does each function doesn't contain more than ~15 lines ?

#### 1.4 Does each function name are descriptive enough ?

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


### 2. Comments

#### 2.1 Are the comments written useful ? 


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



- Does each complex conditions are abstracted ?
- Does the module contains a global description comment 
- No for loop
- Only one level of indentation (max 2)
- Descriptive Variables
- One Liner return if (_.isEmpty($populatedBoxes)) { return; }
- No business logic in the variables
- Defensive IF     if (! compareSlotAvailable()) {
                displayCompareFullMessage();
                return false;
            }
