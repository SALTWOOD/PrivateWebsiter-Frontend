<template>
  <v-app>
    <v-main>
      <router-view />
      <SnackbarManager />
    </v-main>
  </v-app>
</template>

<script setup lang="ts" >
import config from "../package.json";
import axios from "axios";
import { Shared } from "@/types/Shared";
import { UserEntity } from "./types/UserEntity";

const about = String.raw`
This project is created by SaltWood for his own use.
Any non-commercial use is welcome, although this project was not created for others.
Current version: ${config.version}.

PrivateWebsite is a personal website.

This software follows the MIT license.
Copyright (c) 2024 SaltWood.

https://github.com/SALTWOOD/PrivateWebsite-Frontend
https://github.com/SALTWOOD/PrivateWebsite-Backend
`;
// @ts-ignore
window.about = () => console.log(about);

const tip = String.raw`
/*
 *      ____            _       __     __         _ __     
 *     / __ \          | |     / /__  / /_  _____(_) /____ 
 *    / /_/ /  ______  | | /| / / _ \/ __ \/ ___/ / __/ _ \
 *   / ____/  /_____/  | |/ |/ /  __/ /_/ (__  ) / /_/  __/
 *  /_/                |__/|__/\___/_.___/____/_/\__/\___/ 
 *                       v${config.version} | Created by SaltWood
 *                       Type "about()" for more information.
 */
`;
console.log(tip);
console.log("你怎么那么无聊来看浏览器的控制台？这里可啥都没有哦。");
console.log("不过……这句话咋那么熟悉呢……？");

// 拉取信息
axios.get('/api/site/info')
  .then(response => {
    if (response.status === 200) {
      Shared.info.value = response.data;
    }
  })
  .catch((error) => {
    console.error("Failed to fetch custom website information.");
    console.error(error);
  });


axios.get('/api/user')
  .then(response => {
    // 判断响应
    if (response.status !== 200) {
      return;
    }
    // 登录成功后处理数据
    const data = response.data as UserEntity;
    Shared.currentUser = data;
  })
  .catch((error) => {
    console.error("Failed to fetch user information.");
    console.error(error);
  });
</script>
