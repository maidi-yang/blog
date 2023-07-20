---
title: CakePHP recursive 和contain使用
date: 2022-12-07 16:16:45
tags: [cakephp]
categories: [技术,cakephp]
---
正常使用关联查询表与表已经有关联的情况可使用recursive
一般情况下mode方法都会有recursive的值传递,可在调用的时候传递，或者自己写查询方法设置参数。
关于recursive查询的范围：
-1  ：model

0   ：model + belongTo + hasOne

1   ：model + belongTo + hasOne + hasMany

2:  ：model + belongTo + hasOne + hasMany + hasAndBelongsToMany

mode方法一般都会有recursive参数传递：
````
public function findActiveById($id, $recursive = -1) {
   $data= $this->find('first', array(
            'conditions' => array(
                           'id' => $id,
                           'status' => '',
                           ),
            'recursive' => $recursive
            )
         );
   return ($data);
}
````
或者自己使用参数传递：
````
$this->Model->find('All', array(
                              'fields' => array('id'),      
                              'recursive' => -1,
                              )
                           );

````
当使用recursive关联查询时候可以使用unbindModel方法解除一些关联，可以减少一些不用的数据查询出来：
````
$this->Model->unbindModel( array('hasMany' => array('OutOutletGroupLookup')) );
````
当需要解除的关联关系过多时，可使用unbindAll解除所有的关联，然后unbindAll方法内填入需要保留的关联:
````
$this->Model->unbindAll(array('hasMany' => array('OutOutlet')));
````
Contain：
关于contain的正常使用和recursive是差不多的作用，当contain的值等于false的时候作用和recursive = -1一样，只会查询出本表数据，不会查询关联表数据。
使用之前先加载行为：
````
$this->Model->Behaviors->load('Containable'); //加载'Containable'行为

$this->Model->find('all', array(
      'contain' => false, //作用和recursive' => -1一样，不会查询出其他关联表数据
   )
);

````
当需要指定查询某个表的时候就可以在contain当中放入需要的表名：
````
$this->Model->find('all', array(
      'contain' => array('OutOutlet'),//只查询出想要关联的表
   )
);

````
当需要指定查询关联表某一些字段，不希望全部查询出来占用PHP内存的时候就可以在contain的表名参数里面再放入需要的字段名：
````
$this->Model->find('all', array(
      'contain' => array( //使用'contain'在关联表当中查询需要的字段
         'OutOutlet' => array('id', 'code') 
      ),
   )
);

````
当然contain也可以加入条件查询，和正常查询一样加入conditions就可以：
````
$outShops = $this->PosDisplayPanel->OutShop->find('all', array(
      'fields' => $fields,
      'conditions' => $conditions,
      'contain' => array(
         'OutOutlet' => array(
            'fields' => array('id', 'code'),
            'conditions' => array(
            'OutOutlet.olet_id' => 1
         ))
      )
   )
);

````
