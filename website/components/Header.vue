<template>
	<div class="golang-top-header">
		<div class="golang-top-box">
			<div class="golang-top-header-left">
				<div class="golang-logo-container">
                    <a href="/">Golang123</a>
				</div>
				<div class="golang-header-search">
					<form @submit.prevent="onSearch" action="" target="_blank" method="get" class="golang-top-search">
						<input v-model="q" type="text" class="golang-top-input" name="topSearch">
					</form>
				</div>
			</div>
            <div class="golang-top-header-nav">
                <ul>
                    <li><a href="/">首页</a></li>
                    <li><a href="/vote">投票</a></li>
                    <li><a href="/timeline">成长历史</a></li>
                    <li><a href="/about">关于</a></li>
                </ul>
            </div>
			<div class="golang-top-header-right">
				<ul>
                    <li><a href="https://github.com/shen100/golang123" target="_blank">golang123源码</a></li>
                    <li><a href="https://github.com/shen100/golang123/issues" target="_blank">问题反馈</a></li>
                    <template v-if="userData">
                        <Tooltip trigger="hover" title="提示标题" placement="bottom">
                            <a :href="`/user/${user.id}`" class="header-usre-box">
                                <span class="header-avatar">
                                    <img :src="user.avatarURL" alt="">
                                </span>
                                <span class="header-user-name">{{user.name}}</span>
                            </a>
                            <ul slot="content" class="header-user-box">
                                <li><a :href="`/user/${user.id}`">个人首页</a></li>
                                <li><a href="/ac/pwdModify">修改密码</a></li>
                                <li @click="onSignout">退&nbsp&nbsp出</li>
                            </ul>
                        </Tooltip>
                    </template>
					<template v-else>
						<a @click="onSignin"><li>登录</li></a>
                        <a href="/signup"><li>注册</li></a>
					</template>
				</ul>
			</div>
		</div>
	</div>
</template>

<script>
    import request from '~/net/request'
    import ErrorCode from '~/constant/ErrorCode'
    import '~/utils/bd'

    export default {
        props: [
            'user'
        ],
        data () {
            return {
                q: '',
                userData: this.user
            }
        },
        methods: {
            onSearch () {
                let searchURL = 'http://zhannei.baidu.com/cse/search?s=2990237584871814305&entry=1&q=' + encodeURIComponent(this.q)
                window.open(searchURL)
            },
            onSignin () {
                location.href = '/signin?ref=' + encodeURIComponent(location.href)
            },
            onSignout () {
                request
                    .logout()
                    .then(res => {
                        if (res.errNo === ErrorCode.SUCCESS) {
                            this.userData = null
                            window.location.href = '/signin'
                        }
                    })
                    .catch(err => {
                        console.log(err)
                    })
            }
        },
        mounted () {
        }
    }
</script>

<style>

</style>
