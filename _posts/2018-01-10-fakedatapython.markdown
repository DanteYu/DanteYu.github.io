---
layout:     post
title:      Fake data的使用和产生
subtitle:   Python篇
date:       2018-01-10 12:00:00
author:     DanteYu
tags:
    - TestData
---

#### 什么是Fake data
Fake data顾名思义假数据，是在真实产品数据无法使用的情况下，产生地接近于产品环境的数据，多用于开发和测试。

#### Fake data的使用场景
有哪些开发或测试场景会使用fake data？
1. 当你需要开发一个UI原型，但是API还没开发完成继而无法获取相关数据来显示到前端，这个时候，就可以使用mock data来模拟API，从而不阻碍UI的开发工作且使UI和API的开发并行，也有可能提早发现一些问题
2. 当需要产生大量的数据填充数据库的时候，可以使用自动化填充接近于产品数据的fake data到数据库来满足开发测试需求
3. 当需要大量类产品环境数据进行压力测试的时候
4. 单元测试需要产生dummy data的时候

#### Fake data的原则
除了刻意设计的破坏性的test data，我们需要的test data应该是接近于产品环境和现实生活的，而不是固定的搭配。接近于产品数据的fake data能够更好地揭露产品环境潜在的问题，让产品看起来具有真实的使用价值和意义。

#### Fake data的实现方式

在我目前工作的项目中，需要填写各种各样的表单，这些表单收集中不同的用户数据，如果每一次我都填不同的数据来测试，是不是会更接近于产品的真实使用情况，说不定还能挖掘出一些潜在问题。产品环境的数据由于安全隐私不能触碰，那么如何产生大量的假数据呢？主要有下面两种
* 在线服务。比如[mock](https://www.mockaroo.com/)、[fakename](http://www.fakenamegenerator.com/)、[randomuser](https://randomuser.me/)和[randomapi](https://randomapi.com/)
* 各类编程语言的库

这篇文章会介绍Python的四个用于产生fake data的module
* [lipsum](https://github.com/thanethomson/lipsum) - is a simple Lorem Ipsum generator library which can be used in your Python applications
* [radar](https://pypi.python.org/pypi/radar) - Random date generation
* [mimesis](https://lk-geimfari.github.io/mimesis/) - is a fast and easy to use library for Python programming language, which helps generate mock data for a variety of purposes in a variety of languages
* [Faker](http://faker.readthedocs.io/en/master/) - is a Python package that generates fake data for you

#### 安装Python3
在开始之前，先升级下python吧，官方都说了
> Python 2.x is legacy, Python 3.x is the present and future of the language

况且很多流行库比如`numpy`都会不在继续维护python2，继而开始在python3上开发维护。那还有什么理由坚持python2呢？

想看python2还有多久退休，请参考[这里](https://pythonclock.org/)。

我现在的Python开发环境还是macOS自带`python 2.7.10`，所以需要通过Homebrew去安装python3.具体的教程可以参考[这里](https://wsvincent.com/install-python3-mac/)还有[这里](http://docs.python-guide.org/en/latest/starting/install3/osx/)。
```
┌─[diyu@CNdiyu] - [~] - [Wed Jan 10, 16:14]
└─[$] <> python3 --version
Python 3.6.4
```
大功告成！

#### Lorem Ipsum 乱数假文
[lipsum](https://github.com/thanethomson/lipsum)是一个随机文本语句和片段生成器。生成的文本有意义的[lorem ipsum](https://en.wikipedia.org/wiki/Lorem_ipsum)文本。

代码非常简单
```python
import lipsum

print("generate 10 words")
print(lipsum.generate_words(10))

print("*" * 50)
print("generate 3 sentences")
for x in lipsum.generate_sentences(3).split('.'):
    print(x.strip())

print("*" * 50)
print("generate 3 paras")
for x in lipsum.generate_paragraphs(3).split('\n'):
    print(x)
```
输出为
```
generate 10 words
Quae cum dixissem, magis ut illum provocarem quam ut ips!
**************************************************
generate 3 sentences
Hunc vos beatum; ratio quidemvestra sic cogit
At ego quem huic anteponam non audeo dicere;dicet pro me ipsa virtus necdubitabit isti vestro beato M
Regulumanteponere, quem quidem, cum sua voluntate, nulla vi coactuspraeter fidem, quamdederat hosti, ex patria Karthaginemrevertisset, tum ipsum, cum vigiliis et fame cruciaretur, clamatvirtus beatioremfuisse quam potantem in rosa Thorium

**************************************************
generate 3 paras
Atque haec quidem de rerum nominibus. de ipsis rebus autem saepenumero, Brute, vereor ne reprehendar, cum haec ad te
scribam, qui cum in philosophia, tum in optimo genere philosophiae tantum processeris. quod si facerem quasi te
erudiens, iure reprehenderer. sed ab eo plurimum absum neque, ut ea cognoscas, quae tibi notissima sunt, ad te mitto,
sed quia facillime in nomine tuo adquiesco, et quia te habeo aequissimum eorum studiorum, quae mihi communia tecum sunt,
existimatorem et iudicem. attendes igitur, ut soles, diligenter eamque controversiam diiudicabis, quae mihi fuit cum
avunculo tuo, divino ac singulari viro. nam in Tusculano cum essem vellemque e bibliotheca pueri Luculli quibusdam
libris uti, veni in eius villam, ut eos ipse, ut solebam, depromerem. quo cum venissem, M. Catonem, quem ibi esse
nescieram, vidi in bibliotheca sedentem multis circumfusum Stoicorum libris. erat enim, ut scis, in eo aviditas legendi,
nec satiari poterat, quippe qui ne reprehensionem quidem vulgi inanem reformidans in ipsa curia soleret legere saepe,
dum senatus cogeretur, nihil operae rei publicae detrahens. quo magis tum in summo otio maximaque copia quasi helluari
libris, si hoc verbo in tam clara re utendum est, videbatur. quod cum accidisset ut alter alterum necopinato
videremus, surrexit statim. deinde prima illa, quae in congressu solemus: Quid tu, inquit, huc? a villa enim, credo, et:
Si ibi te esse scissem, ad te ipse venissem.

Heri, inquam, ludis commissis ex urbe profectus veni ad vesperum. causa autem fuit huc veniendi ut quosdam hinc libros
promerem. et quidem, Cato, hanc totam copiam iam Lucullo nostro notam esse oportebit; nam his libris eum malo quam
reliquo ornatu villae delectari. est enim mihi magnae curae - quamquam hoc quidem proprium tuum munus est - ut ita
erudiatur, ut et patri et Caepioni nostro et tibi tam propinquo respondeat. laboro autem non sine causa; nam et avi eius
memoria moveor - nec enim ignoras, quanti fecerim Caepionem, qui, ut opinio mea fert, in principibus iam esset, si
viveret - et Lucullus mihi versatur ante oculos, vir cum virtutibus omnibus excellens, tum mecum et amicitia et omni
voluntate sententiaque coniunctus.

Praeclare, inquit, facis, cum et eorum memoriam tenes, quorum uterque tibi testamento liberos suos commendavit, et
puerum diligis. quod autem meum munus dicis non equidem recuso, sed te adiungo socium. addo etiam illud, multa iam mihi
dare signa puerum et pudoris et ingenii, sed aetatem vides.
```
类似[loripsum.net](https://loripsum.net/)这样网站也提供在线生成服务。

#### radar 随机日期生成
[radar](https://pypi.python.org/pypi/radar)用来生成时间非常方便。

代码也非常简单

```python
import radar
import datetime

#随机日期
print(radar.random_date())
#随机日期+时间
print(radar.random_datetime())
#随机时间
print(radar.random_time())
#指定范围随机日期
print(radar.random_date(
start=datetime.datetime(year=1985, month=1, day=1),
stop=datetime.datetime(year=1989, month=12, day=30)))
#指定范围随机日期+时间
print(radar.random_datetime(
start=datetime.datetime(year=1985, month=1, day=1),
stop=datetime.datetime(year=1989, month=12, day=30)))
#指定范围随机时间
print(radar.random_time(
start="2018-01-10T09:00:10",
stop="2018-01-10T18:00:00"))
#radar默认使用python-dateutil库来解析日期，但是这个库非常heavy，可以选择使用轻量级的radar.utils.parse(快5倍)
print(radar.random_datetime(
start="2018-01-10T09:00:10",
stop="2018-01-10T18:00:00",
parse=radar.utils.parse))
#radar.utils.parse usage
start = radar.utils.parse('2018-01-01')
stop = radar.utils.parse('2018-01-05')
print(radar.random_datetime(start=start, stop=stop))
```
输出为
```
2011-07-01
1997-10-25 16:59:15
04:45:21
1985-01-21 18:16:57
1988-06-27 02:49:24
12:49:16
2018-01-10 16:26:11
2018-01-02
```

#### Mimesis 产生mock data
[mimesis](http://mimesis.readthedocs.io/)提供了各类各样数据。这些数据涉及到十几种真实使用场景，比如Dummy data about transport (truck model, car etc.), Personal data (name, surname, age, email etc.), Payment data (credit_card, credit_card_network etc.)。

使用`Mimesis`首先要确定locale，Mimesis支持多达33种不同的[语言](https://github.com/lk-geimfari/mimesis#locales)，下面列子展示了德文和中文数据。

```python
from mimesis import Personal
person_en = Personal('en')
print(person_en.full_name())
print(person_en.age())
print(person_en.favorite_movie())
print("*" * 20)
person_zh = Personal('zh')
print(person_zh.full_name())
print(person_zh.age())
print(person_zh.favorite_movie())
```
输出为
```
Karoline Schneider
33
21
********************
香茗 米
22
星际迷航3:超越星辰
```
本文都以介绍英文为主

之前提到`Mimesis`提供多个不同的data provider来产生不同类别的数据，下面介绍一些常用的provider以及基本使用方法。更多的providers请参考官方网站。

```python
from mimesis import Personal, Address, Business, Payment, Text, Food
from mimesis.enums import Gender

person = Personal('en')
#可以传递性别给full_name()
print(person.full_name(Gender.MALE))
print(person.level_of_english())
print(person.nationality())
print(person.work_experience())
print(person.political_views())
print(person.worldview())

#自定义名字pattern
# pattern 可以有 ('U-d', 'U.d', 'UU-d', 'UU.d', 'UU_d', 'U_d', 'Ud', 'l-d', 'l.d', 'l_d', 'ld', 'default')
templates = ['l-d', 'U-d']
for item in templates:
    print(person.username(template=item))

address1 = Address('en')
print(address1.coordinates())
print(address1.city())

business1 = Business('en')
print(business1.company())
print(business1.company_type())

payment1 = Payment('en')
print(payment1.paypal())
print(payment1.credit_card_expiration_date())

#mimesis也可以生成文字
text1 = Text('en')
print(text1.alphabet())
print(text1.answer())
print(text1.quote())
print(text1.title())
print(text1.word())
print(text1.words())
print(text1.sentence())

food1 = Food('en')
print(food1.drink())
print(food1.fruit())
print(food1.spices())
```

`Generic(*args, **kwargs)`方法提供了统一的接口，所有的provider都可以从这个方法进入
```python
from mimesis import Generic

g = Generic('en')
print(g.food.fruit())
print(g.address.postal_code())
```

如果你想使用自己的数据，想定制化一下，可以自定义类，通过类属性和方法输出数据。
```python
g1 = Generic('en')

class oneProvider(BaseProvider):
    name = "dante"

    class Meta:
        name = "oneprovider"

    def get_age(self):
        return "31"

g1.add_provider(oneProvider)

print(g1.oneprovider.get_age())
print(g1.oneprovider.name)
```

如果不想一个一个生成数据，而是想依据schema批量生成多个，可以使用Field对象和Schema对象完成。
```python
field = Field('en')

body = (
    lambda: {
        #field里面的是API名称
        "name" : field('full_name', gender=Gender.FEMALE),
        "age" : field('age'),
        "email" : field('email'),
        "occupation" : field('occupation')
    }
)
schema = Schema(schema=body)
print(schema.create(iterations=1))
```
#### Faker 产生fake data
[Faker](http://faker.readthedocs.io/en/master/)的使用和`Mimesis`很类似。

`Faker`支持多语言。比如下面的列子就会输出默认的`en_US`和中文`zh_CN`
```python
from faker import Faker

fake = Faker()
print(fake.name())
print(fake.address())
print(fake.city())
print("*" * 20)
fake = Faker('zh_CN')
print(fake.name())
print(fake.address())
print(fake.city())
```
输出为
```
eggy Wood
17796 Johnson Fork Apt. 744
Donaldhaven, DC 41460-2738
Cannonland
********************
向鹏
广西壮族自治区梅县大兴夏路w座 617055
杭州市
```
`Faker`提供多个不同的[data provider](http://faker.readthedocs.io/en/master/providers.html)来产生不同类别的数据，下面介绍一些常用的provider以及基本使用方法。
```python
from faker import Faker

fake = Faker()

# lorem ipsum
print(fake.word())
print(fake.text())
print(fake.paragraphs(5))

print(fake.company())
print(fake.credit_card_full())
print(fake.address())
print(fake.phone_number())
print(fake.date() + ' ' + fake.time())
print(fake.profile())
```

`faker.providers`支持创建自定义的provider
```python
from faker import Faker
from faker.providers import BaseProvider

fake = Faker()

class oneProvider(BaseProvider):
    def hello(self):
        return "I am one provider"

fake.add_provider(oneProvider)
print(fake.hello())
```

`Faker`提供了命令行工具也非常方便，具体使用方法和可用参数，请参考[官方文档](http://faker.readthedocs.io/en/master/)
```
└─[0] faker -h
usage: faker [-h] [--version] [-o output] [-l LOCALE] [-r REPEAT] [-s SEP]
             [-i [INCLUDE [INCLUDE ...]]]
             [fake] [fake argument [fake argument ...]]

faker version 0.8.8

positional arguments:
  fake                  name of the fake to generate output for (e.g. profile)
  fake argument         optional arguments to pass to the fake (e.g. the
                        profile fake takes an optional list of comma separated
                        field names as the first argument)

optional arguments:
  -h, --help            show this help message and exit
  --version             show program's version number and exit
  -o output             redirect output to a file
  -l LOCALE, --lang LOCALE
                        specify the language for a localized provider (e.g.
                        de_DE)
  -r REPEAT, --repeat REPEAT
                        generate the specified number of outputs
  -s SEP, --sep SEP     use the specified separator after each output
  -i [INCLUDE [INCLUDE ...]], --include [INCLUDE [INCLUDE ...]]
                        list of additional custom providers to user, given as
                        the import path of the module containing your Provider
                        class (not the provider class itself)

supported locales:

  ar_AA, ar_EG, ar_JO, ar_PS, ar_SA, bg_BG, bs_BA, cs_CZ, de_AT, de_DE, dk_DK, el_GR, en, en_AU, en_CA, en_GB, en_TH, en_US, es, es_ES, es_MX, et_EE, fa_IR, fi_FI, fr_CH, fr_FR, he_IL, hi_IN, hr_HR, hu_HU, id_ID, it_IT, ja_JP, ka_GE, ko_KR, la, lt_LT, lv_LV, ne_NP, nl_BE, nl_NL, no_NO, pl_PL, pt_BR, pt_PT, ru_RU, sk_SK, sl_SI, sv_SE, th_TH, tr_TR, tw_GH, uk_UA, zh_CN, zh_TW

  faker can take a locale as an argument, to return localized data. If no
  localized provider is found, the factory falls back to the default en_US
  locale.

examples:

  $ faker address
  968 Bahringer Garden Apt. 722
  Kristinaland, NJ 09890

  $ faker -l de_DE address
  Samira-Niemeier-Allee 56
  94812 Biedenkopf

  $ faker profile ssn,birthdate
  {'ssn': u'628-10-1085', 'birthdate': '2008-03-29'}

  $ faker -r=3 -s=";" name
  Willam Kertzmann;
  Josiah Maggio;
  Gayla Schmitt;
```

使用`seed()`可以重现之前的随机数据，这样的话，每次运行代码就会产生一样的数据。下面的代码片段会产生一样的结果，而不是每次都是随机数据。
```python
from faker import Faker
fake = Faker()
fake.seed(9527)
print(fake.name())
```
