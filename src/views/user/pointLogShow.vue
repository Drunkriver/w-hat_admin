<!-- 积分日志详细 -->
<template>
    <div class="main">
        <div class="navbar">
            <el-button type="primary" plain @click="$router.push({path: '/admin/control/pointLog/list', query:{ page:$route.query.pointLogPage, id : $route.query.id,userName : encodeURIComponent(state.userName),beforeUrl:($route.query.beforeUrl != undefined ? $route.query.beforeUrl:'')}})">返回</el-button>					
        </div>
        <div class="nav-user clearfix">
            <div class="avatar">
                <el-popover effect="light" trigger="hover" placement="bottom">
                    <template #default>
                        <p >呢称: {{state.currentUser.nickname}}</p>
                        <p>账号: {{state.currentUser.account}}</p>
                    </template>
                    <template #reference>
                        <div class="avatar-wrapper" >
                            <div class="avatar-badge" v-if="state.currentUser.avatarName == null || state.currentUser.avatarName == ''">
                                <el-avatar shape="square" :size="64" :icon="UserFilled"></el-avatar>
                            </div>
                            <div class="avatar-badge" v-if="state.currentUser.avatarName != null && state.currentUser.avatarName != ''">
                                <el-avatar shape="square" :size="64" :src="state.currentUser.avatarPath+'100x100/'+state.currentUser.avatarName"></el-avatar>
                            </div>
                        </div>
                    </template>
                </el-popover>
            </div>
            <div class="userName" title="账号">
                {{state.currentUser.account}}
                <div class="nickname" title="呢称">
                    {{state.currentUser.nickname}}
                        <i class="tag">积分日志详细</i>
                </div>
            </div>
        </div>
        <div class="data-view" >
            <el-row :gutter="10" type="flex">
                <el-col :span="4"><div class="name">模块：</div></el-col>
                <el-col :span="20">
                    <div class="content">
                        <span v-if="state.pointLog.module == 100">发表话题</span>
                        <span v-if="state.pointLog.module == 200">发表评论</span>
                        <span v-if="state.pointLog.module == 300">发表回复</span>
                        <span v-if="state.pointLog.module == 400">积分解锁话题隐藏内容</span>
                        <span v-if="state.pointLog.module == 500">会员卡订单支付</span>
                        <span v-if="state.pointLog.module == 600">充值</span>
                        <span v-if="state.pointLog.module == 700">提交问题</span>
                        <span v-if="state.pointLog.module == 800">提交答案</span>
                        <span v-if="state.pointLog.module == 900">提交答案回复</span>
                        <span v-if="state.pointLog.module == 1000">悬赏积分</span>
                        <span v-if="state.pointLog.module == 1100">采纳答案</span>
                        <span v-if="state.pointLog.module == 1200">调整赏金</span>
                    </div>
                </el-col>
            </el-row>
            <el-row :gutter="10" type="flex">
                <el-col :span="4"><div class="name">操作用户账号：</div></el-col>
                <el-col :span="20">
                    <div class="content">
                        {{state.pointLog.operationAccount}}
                        <el-tag effect="dark"  v-if="state.pointLog.operationUserType==0" class="tag-wrapper">系统</el-tag>
                        <el-tag effect="dark"  v-if="state.pointLog.operationUserType==1" type="success" class="tag-wrapper" >员工</el-tag>
                        <el-tag effect="dark"  v-if="state.pointLog.operationUserType==2" type="info" class="tag-wrapper" >会员</el-tag>
                
                    </div>
                </el-col>
            </el-row>
            <el-row :gutter="10" type="flex">
                <el-col :span="4"><div class="name">参数Id：</div></el-col>
                <el-col :span="20">
                    <div class="content">
                        <span v-if="state.pointLog.module == 100">话题Id：</span>
                        <span v-if="state.pointLog.module == 200">评论Id：</span>
                        <span v-if="state.pointLog.module == 300">回复Id：</span>
                        <span v-if="state.pointLog.module == 400">话题Id：</span>
                        <span v-if="state.pointLog.module == 500">订单号：</span>
                        <span v-if="state.pointLog.module == 600">用户Id：</span>
                        <span v-if="state.pointLog.module == 700">问题Id：</span>
                        <span v-if="state.pointLog.module == 800">答案Id：</span>
                        <span v-if="state.pointLog.module == 900">答案回复Id：</span>
                        <span v-if="state.pointLog.module == 1000">悬赏积分：</span>
                        <span v-if="state.pointLog.module == 1100">采纳答案：</span>
                        <span v-if="state.pointLog.module == 1200">调整赏金：</span>
                        {{state.pointLog.parameterId}}
                    </div>
                </el-col>
            </el-row>
            <el-row :gutter="10" type="flex">
                <el-col :span="4"><div class="name">积分：</div></el-col>
                <el-col :span="20">
                    <div class="content">
                        <span v-if="state.pointLog.pointState == 1">+</span>
                        <span v-if="state.pointLog.pointState == 2">-</span>
                        {{state.pointLog.point}}
                    </div>
                </el-col>
            </el-row>
            <el-row :gutter="10" type="flex">
                <el-col :span="4"><div class="name">时间：</div></el-col>
                <el-col :span="20"><div class="content">{{state.pointLog.times}}</div></el-col>
            </el-row>
            <el-row :gutter="10" type="flex">
                <el-col :span="4"><div class="name">备注：</div></el-col>
                <el-col :span="20"><div class="content">{{state.pointLog.remark}}</div></el-col>
            </el-row>
        </div>
    </div>
</template>
<script lang="ts" setup>
    import { ComponentInternalInstance, getCurrentInstance, onMounted, reactive} from 'vue';
    import pinia from '@/store/store'
    import {useStore} from '@/store'
    import { storeToRefs } from 'pinia';
    import { AxiosResponse } from 'axios';
    import { useRouter } from 'vue-router';
    import { UserFilled } from '@element-plus/icons-vue'
    import { PointLog, User } from '@/types';

    const store = useStore(pinia);
    const { proxy } = getCurrentInstance() as ComponentInternalInstance;
    const router = useRouter();

    const state = reactive({
        currentUser :{} as User,//当前用户
        pointLogId :'',
        pointLog :{} as PointLog,
        
        id :'',
        userName :'',//用户名称
    });


    //查询积分日志详细
    const queryPointLogShow = () => {
      
        proxy?.$axios({
            url: '/control/pointLog/manage',
            method: 'get',
            params: {
                method :"show",
                pointLogId : state.pointLogId,
                id :state.id,
                userName :state.userName,
            },
           // showLoading: false,//是否显示加载图标
            loadingMask:false,// 是否显示遮罩层
        })
        .then((response: AxiosResponse) => {
            if(response){
                const result: any = response.data;
                if(result){
                    let returnValue = JSON.parse(result);
                    if(returnValue.code === 200){//成功
                        let mapData = returnValue.data;
			    		for(let key in mapData){
			    			if(key == "currentUser"){
			    				state.currentUser = mapData[key];
			    				
			    			}else if(key == "pointLog"){
			    				state.pointLog = mapData[key];
			    			}
			    		}
                    }else if(returnValue.code === 500){//错误
                        
                        
                    }
                }
            }
        })
        .catch((error: any) =>{
            console.log(error);
        });
    }

    onMounted(() => {
        //设置缓存
        store.setCacheComponents(String(router.currentRoute.value.name))


        if(router.currentRoute.value.query.pointLogId != undefined && router.currentRoute.value.query.pointLogId != ''){
			state.pointLogId = router.currentRoute.value.query.pointLogId as string;
			
		}

        if(router.currentRoute.value.query.id != undefined && router.currentRoute.value.query.id != ''){
			state.id = router.currentRoute.value.query.id as string;
			
		}
		if(router.currentRoute.value.query.userName != undefined && router.currentRoute.value.query.userName != ''){
			state.userName = decodeURIComponent(router.currentRoute.value.query.userName as string);
			
		}

		queryPointLogShow();

    }) 
</script>