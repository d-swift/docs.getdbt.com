---
title: Test and document your project
id: test-and-document-your-project
---

import Lightbox from '@site/src/components/lightbox';
import LoomVideo from '@site/src/components/loom';
import CloudCore from '@site/src/components/cloudcore';
import FAQ from '@site/src/components/faqs';

## Add tests to your models
Adding [tests](https://docs.getdbt.com/docs/testing) to a project helps validate
that your models are working correctly. In this section, we're going to add some
tests to your dbt project.


<CloudCore>
    <LoomVideo id="86a1e7ed19084810a7903bd31ebd83e0" />
    <LoomVideo id="61693e928ec94100bef33371aab8296a" />
</CloudCore>

1. Create a new YAML file in the `models` directory, named `models/schema.yml`
2. Add the following contents to the file:
```yaml
version: 2

models:
  - name: customers
    columns:
      - name: customer_id
        tests:
          - unique
          - not_null

  - name: stg_customers
    columns:
      - name: customer_id
        tests:
          - unique
          - not_null

  - name: stg_orders
    columns:
      - name: order_id
        tests:
          - unique
          - not_null
      - name: status
        tests:
          - accepted_values:
              values: ['placed', 'shipped', 'completed', 'return_pending', 'returned']
      - name: customer_id
        tests:
          - not_null
          - relationships:
              to: ref('stg_customers')
              field: customer_id

```
3. Execute `dbt test`, and confirm that all your tests passed. Here's the
expected output when using dbt Cloud and the dbt CLI.

<CloudCore>
    <Lightbox src="/img/successful-tests-dbt-cloud.png" title="Passing tests when using dbt Cloud" />
    <Lightbox src="/img/successful-tests-dbt-cli.png" title="Passing tests when using the dbt CLI" />
</CloudCore>

### FAQs
<FAQ src="faqs/structure-a-project" />
<FAQ src="faqs/available-tests" />
<FAQ src="faqs/test-one-model" />
<FAQ src="faqs/failed-tests" />
<FAQ src="faqs/schema-yml-name" />
<FAQ src="faqs/multiple-test-files" />
<FAQ src="faqs/recommended-tests" />
<FAQ src="faqs/when-to-test" />

## Document your models
Adding [documentation](https://docs.getdbt.com/docs/documentation) to your
project allows you to describe your models in rich detail, and share that
information with your team. Here, we're going to add some basic documentation to
our project.

<CloudCore>
    <LoomVideo id="f946321f692747e59bec3b726eccbfd4" />
    <LoomVideo id="230b30756f674bf7ba38311099070d37" />
</CloudCore>

1. Update your `models/schema.yml` file to include some descriptions, such as
those below.
```yaml
version: 2

models:
- name: customers
  description: One record per customer
  columns:
    - name: customer_id
      description: Primary key
      tests:
        - unique
        - not_null
    - name: first_order_date
      description: NULL when a customer has not yet placed an order.

  - name: stg_customers
    description: This model cleans up customer data
    columns:
      - name: customer_id
        description: Primary key
        tests:
          - unique
          - not_null

  - name: stg_orders
    description: This model cleans up order data
    columns:
      - name: order_id
        description: Primary key
        tests:
          - unique
          - not_null
      - name: status
        tests:
          - accepted_values:
              values: ['placed', 'shipped', 'completed', 'return_pending', 'returned']

```

2. [dbt CLI only] Execute `dbt docs generate` to generate the documentation for your project. dbt introspects your project and your warehouse to generate a json file
with rich documentation about your project.
3. [dbt CLI only] Execute `dbt docs serve` to launch the documentation in a local website.

### FAQs
<FAQ src="faqs/long-descriptions" />
<FAQ src="faqs/sharing-documentation" />

Great work ⭐️! You've just built your first dbt project that's tested and documented!

## Extra exercises

<CloudCore>
    <LoomVideo id="384fa15250d44ca3950a06477bc31a89" />
    <LoomVideo id="6db6956adbb04f55b6027c461e9b1792" />
</CloudCore>

* Try writing a test that fails, for example, omit one of the order statuses in
the `accepted_values` list. What does a failing test look like? Can you debug
the failure?
* Try running the tests for one model only. If you grouped your `stg_` models
into a directory, try running the tests for all the models in that directory.
* Try using a [docs block](https://docs.getdbt.com/docs/documentation#section-docs-blocks)
to add a Markdown description to a model.