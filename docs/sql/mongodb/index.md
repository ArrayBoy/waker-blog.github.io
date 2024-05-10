<!--
 * @Author: your name
 * @Date: 2021-08-12 17:30:57
 * @LastEditTime: 2021-11-05 17:57:32
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /blog/docs/sql/mongodb/index.md
-->

# 基础知识

## mac 下安装

[link](https://blog.csdn.net/ZDCCCnb/article/details/119545375?spm=1001.2101.3001.6650.6&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-6.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-6.no_search_link)

## 进入系统目录

cd /usr/local

## 下载

sudo curl -O https://fastdl.mongodb.org/osx/mongo

## 解压

sudo tar -zxvf mongodb-macos-x86_64-5.0.3.tgz

## 重命名为 mongodb 目录

sudo mv mongodb-macos-x86_64-5.0.3/ mongodb

# 将 MongoDB 的二进制命令文件目录（安装目录/bin）添加到 PATH 路径中

sudo mkdir -p /usr/local/mongodb/data/db

sudo mkdir -p /usr/local/mongodb/logs

sudo touch /usr/local/mongodb/logs/mongodb.log
