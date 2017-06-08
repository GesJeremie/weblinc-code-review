# Checklist JS review


### Functions

#### 1. Does each function do one thing and only one thing ?

````javascript
// Bad
// This function is doing a way too much.

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
// Good
function setupCollapse() {
  hideSubMenus();
  registerEventCollapse();
}
```

- Does each anonymous functions are abstracted ?

````javascript
// Bad
function getAdminUsers(users) {
  
  var admins = _.filter(users, function(user) {
    
    return user.role === 'admin';
    
  });

  return admins;
}
````

````javascript
// Good
function getAdminUsers(users) {
  
  var admins = _.filter(users, isAdmin);

  return admins;

}

function isAdmin(user) {
  return user.role === 'admin';
}
````

````javascript
// Bad
function init() {

  $('.chocobo').on('click', function(e) {
    var $chocobo = $(e.currentTarget);

    if ($chocobo.data('can-ride') === 'fuck-yeah-you-can') {
      rideChocobo($chocobo);
    } 

  });

}
````

````
// Good
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


- Does each function don't contain more than ~15 lines ?

- Does each function name are descriptive enough ?
    noProducts / hasNoProducts

- Does each complex conditions are abstracted ?
- Does the module contains a global description comment 
- Are the comments written useful ? 

- No for loop
- Only one level of indentation (max 2)
- Descriptive Variables
- One Liner return if (_.isEmpty($populatedBoxes)) { return; }
- No business logic in the variables
- Defensive IF     if (! compareSlotAvailable()) {
                displayCompareFullMessage();
                return false;
            }
