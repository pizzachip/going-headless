# Is product in stock

Can we show a product in stock in when using the Magento product catalog.

to determine whether a product is in stock on a store or not several data items have to be checked in both 
the stock item object and in the store configuration. 

Below a checklist of the items that have to be checked

1. should stockmanagement follow store conf setting (bool use_config_manage_stock)?
1.1 yes: use store conf object setting
1.2 no: use stock item object setting

2. does this product have stockmanagement (bool manage_stock | store conf value, not retrievable via API)
2.1 no: then it's in stock -> exit true
2.2 yes: continue checking

3. what does the `in stock` flag say (bool in_stock)
3.1 no: product is not in stock -> exit false
3.2 yes: continue checking

4. should backorders follow store conf setting (bool use_config_backorders)?
4.1 yes: use store conf object setting
4.2 no: use stock item object setting

5. does this product allow backorders (bool back_orders | store conf value, not retrievable via API)
5.1 no: continue to check for qty
5.2 yes: skip qty check, we can order anyway

6. what is the quantity of this object
6.1 0| < requested qty: no stock -> exit false
6.2 1| > requested qty: we can sell!

7. should minimal sales quanity follow store conf setting (bool use_config_min_sale_qty)?
7.1 yes: use store conf object setting
7.2 no: use stock item object setting

8. does this product have a minimal sale quantity (bool min_sale_qty | store conf value, not retrievable via API)
8.1 no: product is in stock -> exit true
8.2 yes: check existing qty vs min sale qty

9. is the available quantity of the product more than min sale quantity
9.1 yes: we can sell -> exit true
9.2 no: -> exit false

In case of configurable, grouped or bundled product this should be done multiple times
