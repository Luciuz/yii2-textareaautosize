yii2-textareaautosize
=====================
JQuery Fullcalendar Yii2 Extension
JQuery from: http://arshaw.com/fullcalendar/
Version 2.1.1
License MIT

JQuery Documentation:
http://arshaw.com/fullcalendar/docs/
Yii2 Extension by <philipp@frenzel.net>

A tiny sample can be found here:
http://yii2textareaautosize.beeye.org

[![Latest Stable Version](https://poser.pugx.org/philippfrenzel/yii2-textareaautosize/v/stable.svg)](https://packagist.org/packages/philippfrenzel/yii2-textareaautosize)
[![Build Status](https://travis-ci.org/philippfrenzel/yii2-textareaautosize.svg?branch=master)](https://travis-ci.org/philippfrenzel/yii2-textareaautosize)
[![Code Climate](https://codeclimate.com/github/philippfrenzel/yii2-textareaautosize.png)](https://codeclimate.com/github/philippfrenzel/yii2-textareaautosize)
[![Version Eye](https://www.versioneye.com/php/philippfrenzel:yii2-textareaautosize/badge.svg)](https://www.versioneye.com/php/philippfrenzel:yii2-textareaautosize)
[![License](https://poser.pugx.org/philippfrenzel/yii2-textareaautosize/license.svg)](https://packagist.org/packages/philippfrenzel/yii2textareaautosize)

Installation
============
Package is although registered at packagist.org - so you can just add one line of code, to let it run!

add the following line to your composer.json require section:
```json
  "philippfrenzel/yii2-textareaautosize":"*",
```

And ensure, that you have the follwing plugin installed global:

> php composer.phar global require "fxp/composer-asset-plugin:~1.0"

Changelog
---------

29-11-2014 Updated to latest 2.2.3 Version of the library

Usage
=====

Quickstart Looks like this:

```php

  $events = array();
  //Testing
  $Event = new \yii2textareaautosize\models\Event();
  $Event->id = 1;
  $Event->title = 'Testing';
  $Event->start = date('Y-m-d\TH:m:s\Z');
  $events[] = $Event;

  $Event = new \yii2textareaautosize\models\Event();
  $Event->id = 2;
  $Event->title = 'Testing';
  $Event->start = date('Y-m-d\TH:m:s\Z',strtotime('tomorrow 6am'));
  $events[] = $Event;

  ?>
  
  <?= \yii2textareaautosize\yii2textareaautosize::widget(array(
      'events'=> $events,
  ));
```

Note, that this will only view the events without any detailed view or option to add a new event.. etc.

AJAX Usage
==========
If you wanna use ajax loader, this could look like this:

```php
<?= yii2textareaautosize\yii2textareaautosize::widget([
      'options' => [
        'language' => 'de',
        //... more options to be defined here!
      ],
      'ajaxEvents' => Url::to(['/timetrack/default/jsoncalendar'])
    ]);
?>
```

and inside your referenced controller, the action should look like this:

```php
public function actionJsoncalendar($start=NULL,$end=NULL,$_=NULL){

    \Yii::$app->response->format = \yii\web\Response::FORMAT_JSON;

    $times = \app\modules\timetrack\models\Timetable::find()->where(array('category'=>\app\modules\timetrack\models\Timetable::CAT_TIMETRACK))->all();

    $events = array();

    foreach ($times AS $time){
      //Testing
      $Event = new \yii2textareaautosize\models\Event();
      $Event->id = $time->id;
      $Event->title = $time->categoryAsString;
      $Event->start = date('Y-m-d\TH:m:s\Z',strtotime($time->date_start.' '.$time->time_start));
      $Event->end = date('Y-m-d\TH:m:s\Z',strtotime($time->date_start.' '.$time->time_end));
      $events[] = $Event;
    }

    return $events;
  }
```
