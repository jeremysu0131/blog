---
title: "[Laravel] FromRequest 更新驗證"
date: 2021-10-22 11:54:33
categories: Laravel
tags:
  - Laravel
  - FromRequest
  - Validation
---

# Intro

使用 laravel 內建的驗證工具會發現一個問題，例如更新時某個欄位設定為 `unique` 則那個欄位沒辦法更新

<!-- more -->

## Problem

```php
public function rules(): array
{
    return [
        'name' => 'required|unique:roles',
        'readable_name' => 'required',
        'description' => '',
    ];
}
```

## Solve

透過 `ignore` 的方式可以忽略檢查自己的資料是否跟資料庫中的重複

```php
public function rules(): array
{
    return [
        'name' => [
            'required',
            Rule::unique('roles')->ignore($this->id),
        ],
        'readable_name' => 'required',
        'description' => '',
    ];
}
```
