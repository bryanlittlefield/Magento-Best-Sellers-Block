Magento-Best-Sellers-Block
==========================

Create a block that show the top sold products ( can be set through cms or .xml)


####Create the following template file (/app/design/frontend/default/YourTheme/template/custom/bestseller.phtml):
```php
<?php

/**
 * @author Branko Ajzele | http://activecodeline.com | branko.ajzele@surgeworks.com
 * @license GPL
 */

$totalPerPage = ($this->show_total) ? $this->show_total : 6;
$counter = 1;
$visibility = array(
                      Mage_Catalog_Model_Product_Visibility::VISIBILITY_BOTH,
                      Mage_Catalog_Model_Product_Visibility::VISIBILITY_IN_CATALOG
                  );

$storeId = Mage::app()->getStore()->getId();
$_productCollection = Mage::getResourceModel('reports/product_collection')
                              ->addAttributeToSelect('*')
                              ->addOrderedQty()
                              ->addAttributeToFilter('visibility', $visibility)
                              ->setOrder('ordered_qty', 'desc');
?>

<div class="content-box-header">Bestsellers</div>
<div class="content-box">
<?php foreach($_productCollection as $product): ?>


<?php if($counter <= $totalPerPage): ?>

<?php $productUrl =  $product->getProductUrl() ?>
  <a href="<?php echo $productUrl ?>" title="View <?php echo $product->name ?>">
    <h4><?php echo $product->name ?></h4>
  </a>
  <small><?php echo $this->__('Total sold quantities') ?>: <?php echo (int)$product->ordered_qty ?></small><br />

  <a href="<?php echo $productUrl ?>" title="View <?php echo $product->name ?>">
  <img src="<?php echo $this->helper('catalog/image')->init($product, 'image')->resize(120); ?>" alt="Product image"  />
  </a> <br />
  <p><?php echo $product->short_description ?></p>

<?php endif; $counter++; ?>
<?php endforeach; ?>
</div>
```

####Call Block by local.xml

```xml
<!--
Default layout, loads most of the pages
-->
  <default>
     <!-- Root -->
    <reference name="root">
      <block type="core/template" before="-" name="home-bestsellers" as="home_bestsellers" show_total="3" template="custom/bestseller.phtml"/>
    </reference>
    
    <!-- Header -->    
    <reference name="header">
      <!-- Insert Code Here -->
    </reference>

    <!-- Left Sidebar -->
    <reference name="left">
      <!-- Insert Code Here -->    
    </reference> 

    <!-- Right Sidebar -->
    <reference name="right">
      <!-- Insert Code Here -->
    </reference>

    <!-- Content -->
    <reference name="content">
      <!-- Insert Code Here -->
    </reference>
  </default>
```
Place in Template Using:
```php
<?php echo $this->getChildHtml('home_bestsellers') ?>
```

####Or set block using CMS Page or Static Block using the following:
```
{{block type=”core/template” template=”inchoo/bestseller.phtml”}}
```

