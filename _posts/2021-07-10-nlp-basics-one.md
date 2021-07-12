---
title: پردازش زبان طبیعی(NLP)- مبانی - یک
category: general
tags:    nlp اموزش  
---

این پست رو با یک نقل قول شروع می کنم:
    
    "Motivation often comes after starting, not before. Action produces momentum."


در چند سری پست آینده می خوام یک سری مبانی برنامه نویسی مربوط به پردازش زبان طبیعی رو توضیح بدم، کدها هم در [<u>این ریپو</u>](https://github.com/spacelover1/NLP-with-Python) قرار دارند. در این آموزش فرض شده که شما با مبانی پایتون یا حداقل برنامه نویسی آشنا هستید چون اینجا مبانی، مثل استفاده از لیست و دیگر نوع داده ها آموزش داده نمی شه.

زبانی که در این سری استفاده می شه پایتونه، پس نیازه که پایتون رو نصب کنید. برای محیط برنامه نویسی هم می تونید از [<u>پایچارم</u>](https://www.jetbrains.com/pycharm/download/#section=windows) یا ژوپیتر استفاده کنید. برای استفاده از ژوپیتر باید [<u>آناکوندا</u>](https://docs.anaconda.com/anaconda/) رو نصب کنید. لینک سایت اصلی این برنامه ها رو هم قرار دادم که برای دانلود می تونید استفاده کنید. حتما دقت کنید که هر برنامه رو برای سیستم عامل خودتون دانلود کنید (خود این سایت ها به صورت خودکار سیستم عامل رو شناسایی می کنند).

خب شروع کنیم. تو این قسمت قراره یک فایل متنی رو بخونیم و یک سری مرتب سازی ها روش انجام بدیم. <br/>
فایلی که استفاده می شه در ریپویی که لینکش رو بالا گذاشتم موجوده. برای خوندن فایل باید اول با استفاده از تابع `open()` فایل رو باز کنیم و با تابع `read()` فایل باز شده رو می خونیم و محتواش رو در یک متغیر به اسم `raw_data` می نویسیم. 



	raw_data = open('file_name').read()

خب حالا باید ببینیم محتوای این `raw_data` به چه صورته تا ببینیم نیاز به مرتب سازی داره برای پردازش های بعدی یا نه. فایل رو اگر دیده باشید داده نه ساختاریافته نیست و خیلی هم بدون ساختار نیست، اول هر خط کلمه ham یا spam داره و بعد با یه تب فاصله یک متنی جلوش نوشته شده. بریم چند خط از فایل رو ببینیم:



    raw_data[0:500]

بعد از اجرای این خط 500 کاراکتر اول فایل نمایش داده می شه. همون طور که می بینید محتوای فایل یک سری رشته است که کاراکترهایی مثل `\t` و `\n` داره. اولی فاصلی ای مه باتب ایجاد شده رو نشون می ده و دومی باعث می شه بره خط بعدی.
خب ما الان می خوایم یکم داده رو مرتب کنیم، برای پردازش های بعدی. برای اینکار کاراکتر `\t` رو با `\n` جایگزین می کنیم و بعد با تابع `split` این رشته رو به لیست تبدیل می کنیم.

    parsed_data = raw_data.replace('\t','\n').split('\n')

بریم ببینیم لیست مون چه شکلیه:


    parsed_data[:5]

الان برچسب یا لیبل در خونه های با ایندکس زوج قرار داره و متن پیام در خونه های ایندکس فرد. حالا که یه نظمی تو ساختار ایجاد شده بریم هر کدوم رو تو لیست جدا بنویسیم:

    label_list  = parsed_data[0::2]
    text_list = parsed_data[1::2]

اولی لیست رو از خونه صفر می خونه و یکی درمیون مقادیر رو می ریزه تو لیست لیبلا، یعنی همون خونه های زوج. خط دوم از خونه یک میاد یکی در میون می خونه و باعث می شه خونه های فرد رو بخونه.<br/>
می تونیم این دو لیست رو ببینیم و مطمئن شیم همونطور که می خواستیم شده. اینجا از تابع `print()` استفاده می کنیم چون در غیر اینصورت ژوپیتر فقط خروجی اخرین خط رو می ده.

    print(label_list[:5])
    print(text_list[:5])

حالا باید این دو لیست رو یه جوری با هم ترکیب کنیم تا برای تحلیل های بعدی بتونیم ازشون استفاده کنیم. برای این کار از تابع `DataFrame()` پانداس استفاده می کنیم و در این دیتافریم یک دیکشنری می سازیم. بعد از ایمپورت کتابخونه می تونیم از توابعش استفاده کنیم:


    import pandas as pd
    full_corpus = DataFrame({'label': label_list, 'body': text_list})

احتمالا بعد از اجرای این خط کد اروری مشاهده می کنید که می گه ارایه ها باید طولشون یکسان باشه. بریم طول این دو لیست رو چک کنیم:


    print(len(label_list))
    print(len(text_list))


همون طور که می بینید طول لیست لیبل یه دونه بیشتر از متنه. بریم چند تا خونه اخر هر دو لیست رو ببینیم چه خبره: 


    label_list [-5:]
    text_list[-5:]

خونه اخر لیست لیبلا یه خونه خالیه. پس وقتی این لیست رو می خونیم، خونه اخر رو باید نادیده بگیریم:

    full_corpus = DataFrame({'label': label_list[:-1], 'body': text_list})

ببینیم دیتافریم مون چه شکلیه:

    full_corpus.head()

خب الان دیتامون یکم ساختار پیدا کرده و نسبت به اول خوانایی بهتری داره. <br/>
همون اول که نگاهی به داده انداختیم و `\t` رو دیدیم باید متوجه شیم که این یک فایلیه که عناصرش با تب از هم جدا شدن. برای خوندن این فایل ها می تونیم از تابع `read_csv` استفاده کنیم، مقدار هدر رو هم در اینجا `None` قرار می دیم چون در فایل اصلی ردیفی برای عنوان ستون وجود نداره.

    dataset = read_csv('file_name', sep="\t", header=None)

برای مشاهده هم می تونیم از تابع `head()` استفاده کنیم:

    dataset.head()

همونطور که می بینید دیگه ردیف اولی برای عنوان ستون ها وجود نداره.

در این قسمت یاد گرفتیم که یک دیتاست متنی رو چطوری بخونیم و برای تحلیل های بعدی مرتبش کنیم.


















