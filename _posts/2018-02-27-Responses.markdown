---
layout:     post
title:      Responses学习笔记
subtitle:   
date:       2018-02-27 12:00:00
author:     DanteYu
tags:
    - Responses
    - TestDouble
---
#### Responses
[Responses](https://github.com/getsentry/responses) - A utility for mocking out the Python Requests library，主要用于Web Service mocking。
#### 安装
使用`pip3 install responses`
#### 基本使用
`responses`的使用主要是靠修饰器`@responses.activate`实现setup和teardown，以及`responses.add()`加入期望的返回值

```python
import responses
import requests
from unittest import TestCase
from requests.exceptions import ConnectionError


class Demo(TestCase):

    #进行responses的setup和teardown
    @responses.activate
    def test_basic(self):
        #确定response的详细内容
        responses.add(responses.GET,
                      'https://nbaplayerprofile.com/api/1/kawhi_leonard',
                      json={
                         'team': 'San Antonio Spurs',
                          'personal': {
                              'DOB': '6/29/1991',
                              'Ht': '67',
                              'Wt': '230'
                          }
                      },
                      status=200)

        expected_Kawhi_profile = {
                         'team' : 'San Antonio Spurs',
                          'personal' : {
                              'DOB' : '6/29/1991',
                              'Ht' : '67',
                              'Wt' : '230'
                          }
                      }
        #发送了两次请求
        resp = requests.get('https://nbaplayerprofile.com/api/1/kawhi_leonard')
        resp1 = requests.get('https://nbaplayerprofile.com/api/1/kawhi_leonard')

        #对获得的response进行验证
        self.assertEqual(resp.status_code, 200)
        self.assertEqual(resp1.json(), expected_Kawhi_profile)
        #responses.calls会记录所有的请求响应情况，可以利用起来进行验证
        self.assertEqual(len(responses.calls), 2)
        self.assertEqual(responses.calls[0].request.url, 'https://nbaplayerprofile.com/api/1/kawhi_leonard')
        self.assertEqual(responses.calls[1].response.text, '{"team": "San Antonio Spurs", "personal": {"DOB": "6/29/1991", "Ht": "67", "Wt": "230"}}')

        # 错误的请求地址 验证会产生ConnectionError
        with self.assertRaises(ConnectionError):
            requests.get('https://nbaplayerprofile.com/api/1/')

    @responses.activate
    def test_error(self):
        #响应的body设置为Exception()，模拟出错的情况
        responses.add(responses.GET,
                      'https://nbaplayerprofile.com/api/1/error',
                      body = Exception(),
                      status=500
        )
        #验证异常被raise
        with self.assertRaises(Exception):
           requests.get('https://nbaplayerprofile.com/api/1/error')

```

我们可以直接把响应写在`add()`里，也可以按照下面的方式传递：
```python
import responses

responses.add(
    responses.Response(
        method='GET',
        url='http://example.com',
    ),
)
```

#### Dynamic Responses
如果我们不想要返回固定的body，而是根据请求的不同返回不同的body，这个就需要`responses.add_callback()`功能。具体做法很简单，写一个callback方法处理request，然后返回一个tuple(status, headers, body)，然后callback方法作为参数传入到`responses.add_callback()`中，相当于设置好了返回的status code， header和body。

```python
    @responses.activate
    def test_dynamic_responses_text(self):
        #定义callback方法，request请求会作为参数传入方法体，进行处理
        def request_callback(request):
            headers = {}
            return (200, headers, str(request.body) + " this is from dynamic")
        #使用add_callback()定义请求，callback关键参数传入处理方法
        responses.add_callback(responses.POST,
                               "https://nbaplayerprofile.com/api/1/foo",
                                callback = request_callback,
                                content_type = 'text/plain'
                               )

        resp = requests.post("https://nbaplayerprofile.com/api/1/foo", "I am request")
        #验证响应文本是否被动态处理，
        self.assertEqual(resp.status_code, 200)
        self.assertEqual(resp.headers, {'Content-Type': 'text/plain'})
        self.assertEqual(resp.text, "I am request this is from dynamic")

    @responses.activate
    def test_dynamic_responses_json(self):
        #这个例子处理json
        def request_callback(request):
            payload = json.loads(request.body)
            headers = {'User-Agent': 'Firefox/12.0'}
            payload.update({'result': 'pass'})
            resp_body = payload
            return (200, headers, json.dumps(resp_body))

        responses.add_callback(responses.POST,
                               'https://nbaplayerprofile.com/api/1/createplayer',
                               callback=request_callback,
                               content_type='application/json')

        request_json_body = {'name': 'Di', 'gender': 'male'}

        resp = requests.post(
            'https://nbaplayerprofile.com/api/1/createplayer',
            json.dumps(request_json_body)
                             )
        self.assertEqual(resp.json(), {"name": "Di", "gender": "male", "result": "pass"})

        self.assertEqual(len(responses.calls), 1)
        self.assertEqual(responses.calls[0].request.url, 'https://nbaplayerprofile.com/api/1/createplayer')
        self.assertEqual(responses.calls[0].response.text, '{"name": "Di", "gender": "male", "result": "pass"}')
        self.assertEqual(responses.calls[0].response.headers['User-Agent'], 'Firefox/12.0')
```

#### 使用context manager来定义response
除了上面说的`@responses.activate`和`responses.add()`方法外，responses也支持with关键字，作为context manager来定义。

```python
# !/usr/bin/env python
# -*- coding: utf-8 -*-os
import responses
import requests
from unittest import TestCase

#不需要使用@responses.activate
class Demo1(TestCase):

    def test_context_manager(self):
        #使用with关键字和responses.RequestsMock()
        with responses.RequestsMock() as resp:
            #同样使用add()方法定义
            resp.add(resp.GET,
                     'https://nbaplayerprofile.com/api/1/kawhi_leonard',
                     json={
                         'team': 'San Antonio Spurs',
                         'personal': {
                             'DOB': '6/29/1991',
                             'Ht': '67',
                             'Wt': '230'
                         }},
                     status=200,
                     content_type='application/json'
                     )

            expected_Kawhi_profile = {
                         'team' : 'San Antonio Spurs',
                          'personal' : {
                              'DOB' : '6/29/1991',
                              'Ht' : '67',
                              'Wt' : '230'
                          }
                      }

            # 发送了两次请求
            resps = requests.get('https://nbaplayerprofile.com/api/1/kawhi_leonard')
            resps1 = requests.get('https://nbaplayerprofile.com/api/1/kawhi_leonard')

            # 对获得的response进行验证
            self.assertEqual(resps.status_code, 200)
            self.assertEqual(resps1.json(), expected_Kawhi_profile)

            # responses.calls会记录所有的请求响应情况，可以利用起来进行验证
            #这里的responses就是with关键字定义的名称
            self.assertEqual(len(resp.calls), 2)
            self.assertEqual(resp.calls[0].request.url, 'https://nbaplayerprofile.com/api/1/kawhi_leonard')
            self.assertEqual(resp.calls[1].response.text,
                             '{"team": "San Antonio Spurs", "personal": {"DOB": "6/29/1991", "Ht": "67", "Wt": "230"}}')

```

当定义的mock service没有被访问的时候，`AssertionError: Not all requests have been executed`会被raise，这时候需要设置参数`assert_all_requests_are_fired=False`来避免这个异常。
```python
def test_requests_fired(self):
    # 设置assert_all_requests_are_fired来避免AssertionError: Not all requests have been executed`
    with responses.RequestsMock(assert_all_requests_are_fired=False) as resp:
        # 同样使用add()方法定义
        resp.add(resp.GET,
                 'https://nbaplayerprofile.com/api/1/kawhi_leonard',
                 json={
                     'team': 'San Antonio Spurs',
                     'personal': {
                         'DOB': '6/29/1991',
                         'Ht': '67',
                         'Wt': '230'
                     }},
                 status=200,
                 content_type='application/json'
                 )
```

同样的，我们也可以定制化responses，这里需要使用`response_callback`参数。
```python
def test_callback(self):
    #定义response_callback方法，接受一个responses为参数，返回一个responses
    #这个例子中我们给responses加了一个属性
    def response_callback(resp):
        resp.result = 'pass'
        return resp

    with responses.RequestsMock(response_callback=response_callback) as stub:
        stub.add(responses.GET,
                           "https://nbaplayerprofile.com/api/1/foo",
                            body="I am body"
                           )
        resps = requests.get("https://nbaplayerprofile.com/api/1/foo", "I am request")
        self.assertEqual(resps.text, "I am body")
        #验证callback方法写入的新属性
        self.assertEqual(resps.result, "pass")
```
