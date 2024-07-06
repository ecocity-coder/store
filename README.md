2024-07-06_22-33-06.png![alt text](https://github.com/ecocity-coder/store/blob/main/2024-07-06_22-33-06.png)
# store
Анализ продаж в интернет-магазине

import pandas as pd
import io

df = pd.read_csv(io.BytesIO(uploaded['sales_data (3).csv']))
print(df)


print("Типы данных в колонках:")
print(df.dtypes)


print("Наличие дубликатов:")
print(df.duplicated().any())

print("Пропущенные значения:")
print(df.isnull().values.any())

unique_order_ids = len(df['order_id'].unique())
print("Количество заказов:", unique_order_ids)

total_sales = df['total_amount'].sum()
orders_count = len(df['order_id'].unique())
average_check = total_sales / orders_count
print("Общая сумма продаж:", total_sales)
print("Количество заказов:", orders_count)
print("Средний чек:", average_check)

df['order_date'] = pd.to_datetime(df['order_date'])
grouped_data = df.groupby([df['order_date'].dt.month]).agg({'quantity': 'sum', 'total_amount': 'sum'})
grouped_data.index.name = 'Month'
grouped_data.reset_index(inplace=True)
grouped_data.columns = ['Month', 'Total Quantity', 'Total Amount']
print(grouped_data)


grouped_data.to_excel('продажи_по_месяцам.xlsx', index=False)

path_to_file = '/content/продажи_по_месяцам.xlsx'
df1 = pd.read_excel(path_to_file)
df1.head()

# @title Month vs Total Amount

from matplotlib import pyplot as plt
import seaborn as sns
def _plot_series(series, series_name, series_index=0):
  palette = list(sns.palettes.mpl_palette('Dark2'))
  xs = series['Month']
  ys = series['Total Amount']

  plt.plot(xs, ys, label=series_name, color=palette[series_index % len(palette)])

fig, ax = plt.subplots(figsize=(10, 5.2), layout='constrained')
df1_sorted = df1.sort_values('Month', ascending=True)
_plot_series(df_sorted, '')
sns.despine(fig=fig, ax=ax)
plt.xlabel('Month')
_ = plt.ylabel('Total Amount')

print (df.head)

unique_customers = len(df['customer_id'].unique())
print("Всего клиентов:", unique_customers)

customer_counts = df['customer_id'].value_counts()
print("Уникальные_клиенты:")
for customer, count in customer_counts.items():
    print(f"- {customer}: {count}")

grouped_data = df.groupby('customer_id').agg({'quantity': 'sum', 'total_amount': 'sum'})
for customer, (quantity_sum, total_amount_sum) in grouped_data.iterrows():
    print(f"Для клиента {customer}:")
    print(f"- Количество_продаж: {quantity_sum}")
    print(f"- Сумма_продаж: {total_amount_sum}")
    print("-"  *  80)

grouped_data.to_excel('продажи_по_клиентам.xlsx', index=False)

unique_product_ids = df['product_id'].unique()
print(unique_product_ids)


unique_values = df['product_id'].value_counts().to_dict()
for product, count in unique_values.items():
    print(f'{product}: {count}')


df_grouped = df.groupby(['customer_id', 'product_id'])
result = df_grouped['product_id'].count()
result.rename('количество повторных покупок', inplace=True)
print(result)

df_grouped = df.groupby('product_id')
result = df_grouped['quantity'].sum().sort_values(ascending=False)
result.rename('количество покупок каждого товара', inplace=True)
print(result)

grouped_data = df.groupby('product_category').agg({'quantity': 'sum', 'total_amount': 'sum'})
for category, (quantity_sum, total_amount_sum) in grouped_data.iterrows():
    print(f"Категория продукта: {category}")
    print(f"\tВсего продаж по категориям: {quantity_sum}")
    print(f"\tСумма продаж по категориям: {total_amount_sum}")
    print("-"  *  80)
grouped_data.to_excel('продажи_по_категориям.xlsx', index=False)


import matplotlib.pyplot as plt
grouped_data = df.groupby(['product_category']).agg({'total_amount': 'sum'})
plt.bar(grouped_data.index, grouped_data['total_amount'])
plt.xlabel('Категория продукта')
plt.ylabel('Сумма продаж')
plt.title('График продаж по категориям продуктов')
plt.show()
