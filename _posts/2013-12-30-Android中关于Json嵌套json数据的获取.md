---
layout: mypost
title: Android中关于Json嵌套json数据的获取
categories: [Android]
---
以前的项目中要获取新浪微博和腾讯微博的用户互粉数据列表，之前同事用的google的Gson来解析的，但是由于我想单独从一堆数据中获取出2个指定的数据用户昵称和头像地址，所以就单独取出并赋值：
```
       try {

                //新浪微博返回的json数据，新浪的只是单个json，所以就取出一次了

                    JSONObject jsonobject=new JSONObject(json);

                    JSONArray jsonarray=jsonobject.getJSONArray("users");

                    JSONObject tempJSon;

                    for(int i=0;i<jsonarray.length();i++){

                          //对取到的JSONObject对象进行处理

                          tempJSon = (JSONObject) jsonarray.get(i);

                          WeiboListData weiboListData = new WeiboListData();

                          weiboListData.setWeiboName(tempJSon.getString("screen_name"));

                          weiboListData.setWeiboIcon(tempJSon.getString("profile_image_url"));

                          weiboListData.setIsChecked(false);

                          itemList.add(weiboListData);

                    }

                    listView.setAdapter(weiboListAdapter);

                    stopProgress();//成功后停止加载

               } catch (Exception e) {

                   // TODO: handle exception

               }
```
本来我以为腾讯微博也一样，结果发现腾讯微博的是一个json套一个json然后才在里面找到对应的昵称和头像数据

试了几种方法都搞不定，网上搜了一圈也没搞定，都瞎说，费了一点事，后来终于搞定了
```
        try {

                    //腾讯微博的传送json属于嵌套的，第一层获取data

                    JSONObject jsonobject=new JSONObject(json);

                    String info = jsonobject.getString("data");

                    //第二层获取info 对应的json数组

                    JSONObject jsonobject2=new JSONObject(info);

                    JSONArray jsonarray=jsonobject2.getJSONArray("info");

                    JSONObject tempJSon;

                    for(int i=0;i<jsonarray.length();i++){

                          //对取到的JSONObject对象进行处理

                          tempJSon = (JSONObject) jsonarray.get(i);

                          WeiboListData weiboListData = new WeiboListData();

                          weiboListData.setWeiboName(tempJSon.getString("nick"));

                          weiboListData.setTweiboName(tempJSon.getString("name"));

                          weiboListData.setWeiboIcon(tempJSon.getString("headurl"));

                          weiboListData.setIsChecked(false);

                          itemList.add(weiboListData);

                    }

                    listView.setAdapter(weiboListAdapter);

                    stopProgress();//停止加载

               } catch (Exception e) {

                   // TODO: handle exception

               }
```
看起来特简单，但是当时脑子短路了，对json也太陌生，所以调了一下午，真是悲剧！如果有n层，就每层照着取出就可以了，

如果碰到第二层为数组的，那就for循环读取，再执行……………………