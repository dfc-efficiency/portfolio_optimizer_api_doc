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
  "UserId": 1,
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
  "UserId": 1,
  "Id": 1
}
```

This endpoint creates a portfolio if it does not already exist for the given (UserId, Name).

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
          "DurationMonths": 6,
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

This endpoint returns all portfolios created by the user.

### HTTP Request

`POST /v1/portfolio_optimizer/portfolio/:UserId`

### URL Parameters

Parameter | Required  | Description
--------- |-----------| -----------
UserId | required  | User's ID


# Project

## Create a project

```python
import requests
data = {
  "PortfolioID": 1,
  "BusinessUnit": "DataDepartment",
  "Name": "RefactoDatalake",
  "Reference": "R01",
  "DurationMonths": 6,
  "CostEuros": 50000,
  "PriorizationIndex": None,
  "IsMandatory": False
}
requests.post(
  '{base_url}/v1/portfolio_optimizer/project',
  json=data
)
```

> Returns JSON structured like this:

```json
{
  "Status": 201,
  "PortfolioID": 1,
  "Id": 1,
  "Name": "RefactoDatalake"
}
```

This endpoint creates a project if it does not already exist for the given (PortfolioID, Name).

### HTTP Request

`POST /v1/portfolio_optimizer/portfolio`

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
PortfolioID | required | Portfolio ID the project belongs to
BusinessUnit | optional | Project's business unit
Name | required | Project's name
Reference | required | Project's reference
DurationMonths | required | Project's duration in months
CostEuros | required | Project's cost in euros
PriorizationIndex | optional | Project's priorization index (the lowest the index is, the sooner this project should be planned in the roadmap)
IsMandatory | required | Is project mandatory (if True, a candidate roadmap must plan this project to be valid)


## Get a project

```python
import requests
project_id = 1
requests.get(
  '{base_url}/v1/portfolio_optimizer/project/:{project_id}'
)
```

> Returns JSON structured like this:

```json
{
    "Id": 1,
    "BusinessUnit": "DataDepartment",
    "Name": "RefactoDatalake",
    "Reference": "R01",
    "DurationMonths": 180,
    "CostEuros": 50000,
    "PriorizationIndex": null,
    "IsMandatory": false,
    "CreatedAt": "2022-12-05 11:10:56",
    "ProjectEvaluation": [
      {
        "ProjectMetricName": "created_value",
        "Value": 500000
      },
      {
        "ProjectMetricName": "risk",
        "Value": 8
      }
    ],
  "ProjectStartingConstraint": {
    "Date": "2023-10-01",
    "Type": "Before"
  },
  "ProjectsConstraint": [
    {
      "ProjectAID": 1,
      "ProjectBID": 32,
      "Type": "NoOverlap",
      "MonthIndexDelay": 2 
    }
  ],
  "ProjectResourceNeeds": [
    {
      "ResourceTypeID": 5,
      "MonthIndex": 1,
      "Need": 60,
      "NeedUnit": "JH"
    }
  ]
}
```

This endpoint returns information about a project.

### HTTP Request

`GET /v1/portfolio_optimizer/project/:ProjectId`

### URL Parameters

Parameter | Required  | Description
--------- |-----------| -----------
ProjectId | required  | Project's ID


## Create a project metric

```python
import requests
data = {
  "Name": "risk",
  "RangeMin": 0,
  "RangeMax": None
}
requests.post(
  '{base_url}/v1/portfolio_optimizer/project_metric',
  json=data
)
```

> Returns JSON structured like this:

```json
{
  "Status": 201,
  "Id": 1,
  "Name": 1
}
```

This endpoint creates project metric if does not already exist for the given (Name)

### HTTP Request

`POST /v1/portfolio_optimizer/project_metric`

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
Name | required | Metric's name
RangeMin | required | Minimum value the metric can take (if null, means - infinity)
RangeMax | required | Maximum value the metric can take (if null, means + infinity)



## Evaluate a project

```python
import requests
data = {
  "ProjectId": 1,
  "ProjectMetricId": 1,
  "Value": 8
}
requests.post(
  '{base_url}/v1/portfolio_optimizer/project_evaluation',
  json=data
)
```

> Returns JSON structured like this:

```json
{
  "Status": 201,
  "ProjectId": 1,
  "ProjectMetricId": 1
}
```

This endpoint creates project evaluation if it does not already exist for the given (ProjectId, ProjectMetricId)

### HTTP Request

`POST /v1/portfolio_optimizer/project_evaluation`

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
ProjectId | required | Project Id to evaluate
ProjectMetricId | required | Metric Id that evaluates the project
Value | required | Project evaluation value using the metric


## Create a project constraint

```python
import requests
data = {
  "ProjectAId": 1,
  "ProjectBId": 32,
  "Type": "NoOverlap",
  "MonthlyIndexDelay": 2
}
requests.post(
  '{base_url}/v1/portfolio_optimizer/project_constraint',
  json=data
)
```

> Returns JSON structured like this:

```json
{
  "Status": 201,
  "ProjectAId": 1,
  "ProjectBId": 32,
  "Type": "NoOverlap",
  "MonthlyIndexDelay": 2
}
```

This endpoint creates a project constraint if it does not already exist for the given (ProjectAId, ProjectBId).

### HTTP Request

`POST /v1/portfolio_optimizer/project_constraint`

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
ProjectAId | required | Project A ID (warning: project roles are not symmetric)
ProjectBId | required | Project B ID (warning: project roles are not symmetric)
Type | required | Type of constraint (NoOverlap)
MonthlyIndexDelay | required | Number of months between two projects overlap


## Create a project starting constraint

```python
import requests
data = {
  "ProjectId": 1,
  "Date": "2023-10-01",
  "Type": "Before"
}
requests.post(
  '{base_url}/v1/portfolio_optimizer/project_starting_constraint',
  json=data
)
```

> Returns JSON structured like this:

```json
{
  "Status": 201,
  "ProjectId": 1,
  "Date": "2023-10-01",
  "Type": "Before"
}
```

This endpoint creates a project starting constraint.

### HTTP Request

`POST /v1/portfolio_optimizer/project_starting_constraint`

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
ProjectId | required | Project ID holding the constraint
Date | required | Date reference
Type | required | Type of constraint (exact, before, after)



# Resources

## Create a resource type

```python
import requests
data = {
  "PortfolioId": 1,
  "Name": "data_engineer",
  "TotalAvailability": 500,
  "AvailabilityUnit": "JH"
}
requests.post(
  '{base_url}/v1/portfolio_optimizer/resource_type',
  json=data
)
```

> Returns JSON structured like this:

```json
{
  "Status": 201,
  "PortfolioId": 1,
  "Name": "data_engineer",
  "ResourceTypeId": 1
}
```

This endpoint creates a resource type if it does not already exist for the given (PortfolioId, Name).

### HTTP Request

`POST /v1/portfolio_optimizer/resource_type`

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
PortfolioId | required | Portfolio ID for which the resource type is created
Name | required | Resource type's name
TotalAvailability | required | Resource total availability for the all portfolio
AvailabilityUnit | required | Unit for availability (JH, days, months)


## Create a resource type availability

```python
import requests
data = {
  "ResourceTypeId": 1,
  "Year": 2024,
  "Month": 10,
  "Availability": 10,
  "AvailabilityUnit": "JH"
}
requests.post(
  '{base_url}/v1/portfolio_optimizer/resource_type_availability',
  json=data
)
```

> Returns JSON structured like this:

```json
{
  "Status": 201,
  "ResourceTypeId": 1,
  "Year": 2024,
  "Month": 10
}
```

This endpoint creates a resource availability if it does not already exist for the given (ResourceTypeId, Year, Month).

### HTTP Request

`POST /v1/portfolio_optimizer/resource_type_availability`

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
ResourceTypeId | required | Resource Type ID
Year | required | Year for the resource availability
Month | required | Resource total availability for the all portfolio
Availability | required | Resource availability for year/month
AvailabilityUnit | required | Unit for availability


## Create a resource need

```python
import requests
data = {
  "ResourceTypeId": 1,
  "ProjectId": 1,
  "MonthIndex": 1,
  "Need": 5,
  "NeedUnit": "JH"
}
requests.post(
  '{base_url}/v1/portfolio_optimizer/resource_type_need',
  json=data
)
```

> Returns JSON structured like this:

```json
{
  "Status": 201,
  "ResourceTypeId": 1,
  "ProjectId": 1,
  "MonthIndex": 1
}
```

This endpoint creates a resource need for a project if it does not already exist for the given (ResourceTypeId, ProjectId, MonthIndex).

### HTTP Request

`POST /v1/portfolio_optimizer/resource_type_need`

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
ResourceTypeId | required | Resource Type ID
ProjectId | required | Project ID for which the resource is needed
MonthIndex | required | Project month index for which the resource is needed
Need | required | Resource need expressed in NeedUnit
NeedUnit | required | Unit of ressource need


# Roadmaps

## Create a roadmap exercise

```python
import requests
data = {
  "PortfolioId": 1,
  "UserId": 1,
  "StartDate": "2023-01-01",
  "MaxStartingMonthIndex": 12
}
requests.post(
  '{base_url}/v1/portfolio_optimizer/roadmap_exercise',
  json=data
)
```

> Returns JSON structured like this:

```json
{
  "Status": 201,
  "Id": 1,
  "PortfolioId": 1,
  "StartDate": "2023-01-01"
}
```

This endpoint creates a new roadmap exercise for the given PortfolioId.

### HTTP Request

`POST /v1/portfolio_optimizer/roadmap_exercise`

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
PortfolioId | required | Portfolio ID for which the roadmap exercise is created
UserId | required | User ID that creates the roadmap exercise
StartDate | required | The roadmap starting date
MaxStartingMonthIndex | required | The maximum month index a project can start (1 -> january ... 12 -> december)


## Create a roadmap candidate

```python
import requests
data = {
  "RoadmapExerciseId": 1,
  "Tag": "Reference roadmap",
  "Projects": [
    {
      "ProjectId": 1,
      "StartingMonthIndex": 5
    },
    {
      "ProjectId": 2,
      "StartingMonthIndex": 7
    }
  ]
}
requests.post(
  '{base_url}/v1/portfolio_optimizer/roadmap_candidate',
  json=data
)
```

> Returns JSON structured like this:

```json
{
  "Status": 201,
  "Id": 1,
  "RoadmapExerciseId": 1,
}
```

This endpoint creates a new roadmap candidate for the given roadmap exercise.

### HTTP Request

`POST /v1/portfolio_optimizer/roadmap_candidate`

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
RoadmapExerciseId | required | Roadmap exercice ID of the roadmap candidate
Tag | optional | Tag description the roadmap



## Get a roadmap candidate

```python
import requests
roadmap_candidate_id = 123
requests.get(
  '{base_url}/v1/portfolio_optimizer/roadmap_candidate/:{roadmap_candidate_id}'
)
```

> Returns JSON structured like this:

```json
{
  "Status": 201,
  "Id": 123,
  "RoadmapExerciseId": 1,
  "Tag": "Reference roadmap",
  "Projects": [
    {
      "ProjectId": 1,
      "StartingMonthIndex": 5
    },
    {
      "ProjectId": 2,
      "StartingMonthIndex": 7
    }
  ],
  "RoadmapCandidateEvaluation": [
    {
      "RoadmapCandidateMetric": "priorization_order",
      "Value": 10
    },
    {
      "RoadmapCandidateMetric": "global_value_creation",
      "Value": 520
    }
  ],
  "IsValid": true,
  "Hash": "57000001",
  "GenerationId": 45,
  "CreatedAt": "2022-10-05 12:41:25"
}
```

This endpoint returns a roadmap candidate.

### HTTP Request

`GET /v1/portfolio_optimizer/roadmap_candidate`


## Create a roadmap candidate evaluation

```python
import requests
data = {
  "RoadmapCandidateId": 1,
  "RoadmapCandidateMetricId": 1,
  "Value": 50
}
requests.post(
  '{base_url}/v1/portfolio_optimizer/roadmap_candidate_evaluation',
  json=data
)
```

> Returns JSON structured like this:

```json
{
  "Status": 201,
  "RoadmapCandidateId": 1,
  "RoadmapCandidateMetricId": 1,
}
```

This endpoint creates a new roadmap candidate evaluation.

### HTTP Request

`POST /v1/portfolio_optimizer/roadmap_candidate_evaluation`

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
RoadmapCandidateId | required | Roadmap candidate Id
RoadmapCandidateMetricId | required | Roadmap candidate metric Id
Value | required | Metric value


# Optimization

## Create a optimization run request

```python
import requests
data = {
  "RoadmapExerciseId": 1,
  "UserId": 1,
  "Config": {"nbr_generations": ...}
}
requests.post(
  '{base_url}/v1/portfolio_optimizer/optimization_run',
  json=data
)
```

> Returns JSON structured like this:

```json
{
  "Status": 201,
  "RoadmapExerciseId": 1,
  "RunIndex": 42
}
```

This endpoint creates a new optimization run for the roadmap exercise.

### HTTP Request

`POST /v1/portfolio_optimizer/optimization_run`

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
RoadmapExerciseId | required | Roadmap Exercise Id
UserId | required | User Id that trigger the run
Config | required | Run config



## Get the status of an optimization run

```python
import requests
roadmap_exercise_id = 1
requests.get(
  '{base_url}/v1/portfolio_optimizer/optimization_run/:{roadmap_exercise_id}',
)
```

> Returns JSON structured like this:

```json
{
  "Id": 5412,
  "RoadmapExerciseId": 1,
  "RunIndex": 42,
  "Config": {"nbr_generations":  ...},
  "Status": "Success",
  "OptimizationEvaluation": [
    {
      "OptimizationMetric": "pareto_first_front_size",
      "Value": 324
    },
    {
      "OptimizationMetric": "generation_mean_computation_time_seconds",
      "Value": 1206
    }
  ],
  "Generation": [
    {
      "GenerationId": 5412,
      "Index": 1,
      "CreatedAt": "2022-10-21 10:59:30"
    }
  ],
  "LastStatusChangedAt": "2022-10-21 12:10:42",
  "CreatedAt": "2022-10-21 10:57:28"
}
```

This endpoint returns the status of an optimization run.

### HTTP Request

`POST /v1/portfolio_optimizer/optimization_run/:{RoadmapExerciseId}:{OptimizationRunId}`

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
RoadmapExerciseId | required | Roadmap Exercise Id of the optimization run
OptimizationRunId | optional | Optimization run ID (returns the one with the highest RunIndex)
