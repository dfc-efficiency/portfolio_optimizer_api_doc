---
title: Portfolio Optimizer API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - python

toc_footers:

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Portfolio Optimizer API
---

# Introduction

This is a documentation for DFC Efficiency product: Portfolio Optimizer.

# Authentication

How to authenticate:


# User and Roles

## Get current user

```python
import requests
requests.get('https://.../portfolio_optimizer/user')
```

> Returns JSON structured like this:

```json
  {
    "Id": 1,
    "FirstName": "John",
    "LastName": "Smith",
    "Email": "john.smith@example.com",
    "Organization": "random_company",
    "RoleName": "role_name"
  }
```

This endpoint retrieves the current authenticated user.

### HTTP Request

`GET /portfolio_optimizer/user`


## Create a user

```python
import requests
data = {
  "RoleID": 1,
  "FirstName": "John",
  "LastName": "Smith",
  "Email": "john.smith@example.com",
  "Organization": "random_company",
  "Password": 1234
}
requests.post(
  '{base_url}/v1/portfolio_optimizer/user',
  json=data
)
```

> Returns JSON structured like this:

```json
{
  "Status": 201,
  "Id": 1
}
```

This endpoint creates a user.

### HTTP Request

`POST /v1/portfolio_optimizer/user`

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
RoleId | required | Role ID associated to the user
FirstName | required | User's first name
LastName | required | User's last name
Email | required | User's email
Organization | required | User's organization name
Password | required | User's password


# Portfolio

## Create a portfolio

```python
import requests
data = {
  "Name": "MyPorfolio"
}
requests.post(
  '{base_url}/v1/portfolio_optimizer/portfolio',
  json=data
)
```

> Returns JSON structured like this:

```json
{
  "Status": 201,
  "Id": 1
}
```

This endpoint creates a portfolio.

### HTTP Request

`POST /v1/portfolio_optimizer/portfolio`

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
Name | required | Portfolio's name


## Get all the portfolios created by a user

```python
import requests
user_id = 1
requests.get(
  '{base_url}/v1/portfolio_optimizer/portolio/:{user_id}'
)
```

> Returns JSON structured like this:

```json
{
  "PortfolioData": [
    {
      "Id": 1,
      "UserId": 1,
      "Projects": [
        {
          "Id": 1,
          "BusinessUnit": "DataDepartment",
          "Name": "RefactoDatalake",
          "Reference": "R01",
          "DurationDays": 180,
          "CostEuros": 50000,
          "PriorizationIndex": null,
          "IsMandatory": false,
          "CreatedAt": "2022-12-05 11:10:56"
        }
      ],
      "CreatedAt": "2022-12-05 10:09:50"
    }
  ]
}
```

This endpoint returns a portfolio.

### HTTP Request

`POST /v1/portfolio_optimizer/portfolio/:user_id`

### URL Parameters

Parameter | Required  | Description
--------- |-----------| -----------
UserId | required  | User's ID


