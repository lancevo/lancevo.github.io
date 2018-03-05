---
layout: post
title:  "Mock Http results for HttpClient"
date:   2018-03-05 17:30:00
categories: angular
tags: 
#image:
published: false
---

Mocking backend for HttpClient doesn't require a lot of set up like Http, and the result of requests can be mocked individually. 


```js
import { HttpClient } from '@angular/common/http';

import {
  HttpTestingController,
  HttpClientTestingModule
} from '@angular/common/http/testing';

import {
  async,
  inject,
  ComponentFixture,
  TestBed
} from '@angular/core/testing';

import { MyService } from './my.service'; 
import { MyComponent} from './my.component';

describe(' mocking httpClient', () => {

  let component: MyComponent;
  let fixture: ComponentFixture<MyComponent>;
  let httpMock: HttpTestingController; 

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [
        HttpClientTestingModule
      ],
      providers: [
        MyService 
      ],
      declarations: [
        MyComponent
      ],
      schemas: [CUSTOM_ELEMENTS_SCHEMA] 
    })
  });
  
  // set up component
  before(async(inject([HttpTestingController], _httpMock: HttpTestingController => {
    fixture = TestBed.createComponent(MyComponent);
    component = fixture.componentInstance;
    fixture.detectChanges();

    httpMock = _httpMock;        
  })));
  
  // Make sure there's no open connection
  afterEach(inject([HttpTestingController], (_httpMock: HttpTestingController) => {
    _httpMock.verify();
  }));
  
  it('get /product', () => {
    // this can be declared more than one
    const getProductReq = httpMock.expectOne('/product/1');
    getProductReq.flush({ name: 'apple'});
    
    const res = component.getProductById(1);
    
    expect(res.name).toBe('apple');
  });

});

```


