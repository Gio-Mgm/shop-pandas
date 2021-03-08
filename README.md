# Requêtes pandas 

## import et préparation des variables

```py
import pandas as pd
import datetime

customers_df      = pd.read_csv("./dataset/olist_customers_dataset.csv")
products_df       = pd.read_csv("./dataset/olist_products_dataset.csv")
sellers_df        = pd.read_csv("./dataset/olist_sellers_dataset.csv")
orders_df         = pd.read_csv("./dataset/olist_orders_dataset.csv")
order_payments_df = pd.read_csv("./dataset/olist_order_payments_dataset.csv")
order_reviews_df  = pd.read_csv("./dataset/olist_order_reviews_dataset.csv")
order_items_df    = pd.read_csv("./dataset/olist_order_items_dataset.csv")
geolocation_df    = pd.read_csv("./dataset/olist_geolocation_dataset.csv")
#category_df = pd.read_csv("./datasets/category.csv")
```

## Nombre de clients total
```py
total_customers = customers_df.customer_id.count()
```

## Nombre de produits total
```py
total_products = products_df.product_id.count()
```
## Nombre de commandes total
```py
total_orders = orders_df.order_id.count()
```
## Nombre de commandes selon leurs états (en cours de livraison etc...)
```py
total_orders_by_status = orders_df.order_status.value_counts()
```
## Nombre de commandes par mois
```py
orders_df["month"] = pd.DatetimeIndex(orders_df.order_purchase_timestamp).month

total_orders_by_month = orders_df.month.value_counts().sort_index()
```
## Panier moyen d'un client
```py
mean_payment = order_payments_df.payment_value.mean()
```
## Score de satisfaction moyen (notation sur la commande)
```py
mean_reviews = order_reviews_df.review_score.mean()
```
## Nombre de vendeurs
```py
total_sellers = sellers_df.seller_id.count()
```
## Nombre de vendeurs par région
```py
total_sellers_by_state = sellers_df.seller_state.value_counts()
```
## Durée moyenne entre la commande et la livraison
```py
delivered = pd.to_datetime(orders_df.order_delivered_customer_date)
purchase = pd.to_datetime(orders_df.order_purchase_timestamp)

mean_time = (delivered - purchase).mean()
```
## Quantité de produit vendu par catégorie
```py
# fusion des tables order_items_df et products_df
merged_order_items = pd.merge(order_items_df,products_df)

sold_by_category = merged_order_items_df.product_category_name.value_counts().sort_index()
```
## Nombre de commande par jours
```py
# création d'une nouvelle colonne date à partir du order_purchase_timestamp
orders_df["date"] = pd.DatetimeIndex(orders_df.order_purchase_timestamp).date

total_orders_by_date = orders_df.date.value_counts().sort_index()
```
## Nombre de commande par ville (ville du vendeur)
```py
# fusion des tables order_items_df et sellers_df
merged_items_seller_df = pd.merge(order_items_df, sellers_df)

total_orders_by_seller_city = merged_items_seller_df.seller_city.value_counts().sort_index()
```
## Prix minimum des commandes
```py
# fusion des tables orders_df et order_payments_df
merged_payment = pd.merge(orders_df, order_payments_df)
# utilisation des tuples avec le statut livré uniquement
merged_payment = merged_payment.loc[merged_payment["order_status"] == "delivered"]

min_order_value = merged_payment.payment_value.min()
```
## Prix maximum des commandes
```py
max_order_value = order_payments_df.payment_value.max()
```
