---
title: 初识SpringBoot
index_img: /img_index/index/20251207-001.png
tags: [JAVA, SpingBoot]
categories: [其他计算机语言]
abbrlink: 36936
date: 2025-12-07 10:49:24
updated: 2025-12-07 10:49:24
---


### 背景

项目上去其他项目支援Java源码开发，对于新的SpringBoot框架不是很了解，去之前也做了很多Java学习，包含基础语法（真应了那句**万物皆对象**）、SpringBoot框架的学习。

<!--more-->
<hr />


通过这几天的B站视频学习，总结记录一下SpringBoot的学习内容。

### 1、SpringBoot项目脚手架

推荐自动化生成脚手架源码：https://start.spring.io/
配置说明：
```
- Project：Maven
- Language：Java
- Spring Boot：4.0.0
- Project Metadata：
  - Group
  - Artifact
  - Name
  - Description
  - Package name
  - Packaging
  - Configuration
  - Java
- Dependencies
  - Spring Web
  - Spring Data JPA
  - Mysql Driver
```

### 2、项目结构

- control：Controller层
- service：Service层
- dao：Modal层
- dto：数据传输模型
- converter：数据转换（一个对象生成新的对象）


### 3、编译器

IDEA

### 4、项目启动

IDEA找到BootDemoApplication（主类）直接启动即可。

### 5、示例代码

#### 5.1、Controller层
```
package top.pygo2.boot_demo.control;

import java.util.List;

import org.springframework.web.bind.annotation.*;
import top.pygo2.boot_demo.dao.Sysuser;
import top.pygo2.boot_demo.dto.SysuserDTO;
import top.pygo2.boot_demo.service.SysuserService;
import top.pygo2.boot_demo.Response;

import org.springframework.beans.factory.annotation.Autowired;


@RestController
@RequestMapping("/sysuser")
public class SysuserControl {

    @Autowired
    private SysuserService sysuserService;


    @GetMapping("/{id}")
    public Response<SysuserDTO> getSysuserById(@PathVariable long id){
        System.out.println("getSysuserById=============id：" + id);
        return Response.success(sysuserService.getSysuserById(id));
    }

    @GetMapping("/rtx/{rtx}")
    public Response<SysuserDTO> getSysuserByRtx(@PathVariable String rtx){
        System.out.println("getSysuserByRtx=============rtx：" + rtx);
        return Response.success(sysuserService.getSysuserByRtx(rtx));
    }

    @GetMapping("")
    public Response<List<SysuserDTO>> getAllSysuser(){
        return Response.success(sysuserService.getSysuserList());
    }

    @PostMapping("")
    public Response<Long> addSysuser(@RequestBody SysuserDTO sysuserDTO){
        return sysuserService.addSysuser(sysuserDTO);
    }

    @DeleteMapping("/{rtx}")
    public Response<String> deleteSysuser(@PathVariable String rtx){
        System.out.println("deleteSysuser=============rtx：" + rtx);
        return sysuserService.deleteSysuser(rtx);
    }


}
```

#### 5.2、Service层
##### Interface
```
package top.pygo2.boot_demo.service;

import top.pygo2.boot_demo.Response;
import top.pygo2.boot_demo.dao.Sysuser;
import top.pygo2.boot_demo.dto.SysuserDTO;
import java.util.List;

public interface SysuserService {
    SysuserDTO getSysuserById(Long id);

    SysuserDTO getSysuserByRtx(String rtx);

    List<SysuserDTO> getSysuserList();

    Response<Long> addSysuser(SysuserDTO sysuserDTO);

    Response<String> deleteSysuser(String rtx);
}
```
##### Service
```
package top.pygo2.boot_demo.service;

import jakarta.transaction.Transactional;
import org.springframework.beans.factory.annotation.Autowired;
import top.pygo2.boot_demo.Response;
import top.pygo2.boot_demo.converter.SysuserConverter;
import top.pygo2.boot_demo.dao.Sysuser;
import top.pygo2.boot_demo.dto.SysuserDTO;
import org.springframework.stereotype.Service;
import top.pygo2.boot_demo.dao.SysuserRepository;
import java.util.List;
import java.util.ArrayList;


@Service
public class SysuserServiceImpl implements SysuserService {

    @Autowired
    private SysuserRepository sysuserRepository;

    @Override
    public SysuserDTO getSysuserById(Long id) {
        if (id == null) { return null; }

        Sysuser result = sysuserRepository.findById(id).orElse(null);
        if (result == null) {return null;}

        for(int i=0; i<=55; i++) {
            System.out.print("*");
        }
        System.out.println();
        System.out.print("getSysuserById：" + result.getName());

        return SysuserConverter.convert(result);
    }

    @Override
    public SysuserDTO getSysuserByRtx(String rtx) {
        if (rtx == null) { return null; }

        Sysuser result = new Sysuser();
        result = sysuserRepository.findByRtx(rtx).orElse(null);
        if (result == null) { return null; }

        for(int i=0; i<=55; i++) {
            System.out.print("*");
        }
        System.out.println();
        System.out.print("getSysuserByRtx：" + result.getName());

        return SysuserConverter.convert(result);
    }

    @Override
    public List<SysuserDTO> getSysuserList() {
        List<SysuserDTO> result = new ArrayList<>();
        for (Sysuser sysuser: sysuserRepository.findAll()) {
            if (sysuser == null) { continue; }
            result.add(SysuserConverter.convert(sysuser));
        }
        return result;
    }

    @Override
    public Response<Long> addSysuser(SysuserDTO sysuserDTO) {
        Sysuser sysusersRtx = sysuserRepository.findByRtx(sysuserDTO.getRtx()).orElse(null);
        if (sysusersRtx != null) {
            return Response.fail("用户Rtx已存在，新增失败");
        }
        List<Sysuser> sysusersListEmail = sysuserRepository.findByEmail(sysuserDTO.getEmail());
        if (sysusersListEmail.size() > 0) {
            return Response.fail("用户邮箱已存在，新增失败");
        }
        List<Sysuser> sysusersListPhone = sysuserRepository.findByPhone(sysuserDTO.getPhone());
        if (sysusersListPhone.size() > 0) {
            return Response.fail("用户电话已存在，新增失败");
        }
        Sysuser sysuser = SysuserConverter.convert(sysuserDTO);
        sysuserRepository.save(sysuser);
        return Response.success(sysuser.getId());
     }

    @Override
    @Transactional
    public Response<String> deleteSysuser(String rtx) {
       if (rtx == null) {
           return Response.fail("缺少rtx参数");
       }
       Sysuser sysuser = sysuserRepository.findByRtx(rtx).orElse(null);
       if (sysuser == null) {
           return Response.fail("用户不存在");
       } else {
           sysuserRepository.deleteByRtx(rtx);
           return Response.success("删除成功");
       }
     }

}
```

#### 5.3、Modal层
##### Sysuser
```
package top.pygo2.boot_demo.dao;

import jakarta.persistence.*;


//@Data
@Entity
@Table(name = "sysuser")
public class Sysuser {

    @Id
    @Column(name = "id")
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    @Column(name = "rtx_id")
    private String rtx;

    @Column(name = "md5_id")
    private String md5;

    @Column(name = "fullname")
    private String name;

    @Column(name = "password")
    private String password;

    @Column(name = "email")
    private String email;

    @Column(name = "phone")
    private String phone;

    @Column(name = "avatar")
    private String avatar;

    @Column(name = "introduction")
    private String introduction;

    @Column(name = "role")
    private String role;

    @Column(name = "department")
    private String department;

    @Column(name = "create_time")
    private String createTime;

    @Column(name = "create_rtx")
    private String createRtx;

    @Column(name = "delete_time")
    private String deleteTime;

    @Column(name = "delete_rtx")
    private String deleteRtx;

    @Column(name = "is_del")
    private String status;

    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getRtx() {
        return rtx;
    }

    public void setRtx(String rtx) {
        this.rtx = rtx;
    }

    public String getMd5() {
        return md5;
    }

    public void setMd5(String md5) {
        this.md5 = md5;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

    public String getAvatar() {
        return avatar;
    }

    public void setAvatar(String avatar) {
        this.avatar = avatar;
    }

    public String getIntroduction() {
        return introduction;
    }

    public void setIntroduction(String introduction) {
        this.introduction = introduction;
    }

    public String getRole() {
        return role;
    }

    public void setRole(String role) {
        this.role = role;
    }

    public String getDepartment() {
        return department;
    }

    public void setDepartment(String department) {
        this.department = department;
    }

    public String getCreateTime() {
        return createTime;
    }

    public void setCreateTime(String createTime) {
        this.createTime = createTime;
    }

    public String getCreateRtx() {
        return createRtx;
    }

    public void setCreateRtx(String createRtx) {
        this.createRtx = createRtx;
    }

    public String getDeleteTime() {
        return deleteTime;
    }

    public void setDeleteTime(String deleteTime) {
        this.deleteTime = deleteTime;
    }

    public String getDeleteRtx() {
        return deleteRtx;
    }

    public void setDeleteRtx(String deleteRtx) {
        this.deleteRtx = deleteRtx;
    }

    public String getStatus() {
        return status;
    }

    public void setStatus(String status) {
        this.status = status;
    }
}
```
##### SysuserRepository
```
package top.pygo2.boot_demo.dao;


import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.List;
import java.util.Optional;

@Repository
public interface SysuserRepository extends JpaRepository<Sysuser, Long> {
    Optional<Sysuser> findByRtx(String rtx);
    List<Sysuser> findByEmail(String email);
    List<Sysuser> findByPhone(String phone);

    Sysuser deleteByRtx(String rtx);
}
```

#### 5.4、DTO
```
package top.pygo2.boot_demo.dto;

public class SysuserDTO {

    private long id;

    private String rtx;

    private String md5;

    private String name;

    private String email;

    private String phone;

    private String avatar;

    private String introduction;

    private String status;

    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getRtx() {
        return rtx;
    }

    public void setRtx(String rtx) {
        this.rtx = rtx;
    }

    public String getMd5() {
        return md5;
    }

    public void setMd5(String md5) {
        this.md5 = md5;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

    public String getAvatar() {
        return avatar;
    }

    public void setAvatar(String avatar) {
        this.avatar = avatar;
    }

    public String getIntroduction() {
        return introduction;
    }

    public void setIntroduction(String introduction) {
        this.introduction = introduction;
    }

    public String getStatus() {
        return "0".equals(status) ? "否" : "是";
    }

    public void setStatus(String status) {
        this.status = status;
    }
}

```

#### 5.4、Converter
```
package top.pygo2.boot_demo.converter;

import top.pygo2.boot_demo.dao.Sysuser;
import top.pygo2.boot_demo.dto.SysuserDTO;
import top.pygo2.boot_demo.Utils;

public class SysuserConverter {

    public static SysuserDTO convert(Sysuser sysuser) {
        SysuserDTO sysuserDTO = new SysuserDTO();
        sysuserDTO.setId(sysuser.getId());
        sysuserDTO.setRtx(sysuser.getRtx());
        sysuserDTO.setMd5(sysuser.getMd5());
        sysuserDTO.setName(sysuser.getName());
        sysuserDTO.setEmail(sysuser.getEmail());
        sysuserDTO.setPhone(sysuser.getPhone());
        sysuserDTO.setAvatar(sysuser.getAvatar());
        sysuserDTO.setIntroduction(sysuser.getIntroduction());
        sysuserDTO.setStatus(sysuser.getStatus());
        return sysuserDTO;
    }

    public static Sysuser convert(SysuserDTO sysuserDTO) {
        Sysuser sysuser = new Sysuser();
        sysuser.setRtx(sysuserDTO.getRtx());
        sysuser.setMd5(Utils.md5(sysuserDTO.getRtx()));
        sysuser.setName(sysuserDTO.getName());
        sysuser.setPassword("abcd.1234");  // 默认密码
        sysuser.setEmail(sysuserDTO.getEmail());
        sysuser.setPhone(sysuserDTO.getPhone());
        sysuser.setAvatar("http://store.pygo2.top/w.jpg"); // 默认头像
        sysuser.setIntroduction(sysuserDTO.getIntroduction());
        sysuser.setStatus("0");
        return sysuser;
    }
}

```