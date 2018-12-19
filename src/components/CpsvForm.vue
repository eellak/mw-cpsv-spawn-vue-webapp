<template id='cpsv-form'>
	<div>

		<div id='username' v-if="isLogged">{{ userName }}</div>
		<form id="login-form" action="" v-if="!isLogged && availableFields">
			<input type="text" v-model="userName">
			<input type="password" v-model="password">
			<!-- <div v-html="availableFields"></div> -->
			<!-- <input v-bind:type="key==='password'?'password':'text'" v-for="(value, key) in availableFields" v-bind:name="key" v-model="key==='password'?password:userName"> -->
			<button type="submit" @click="login()">Log in</button>
			<!-- <textarea v-bind:value="field_name"> -->
			<!-- </textarea> -->
		</form>
		<form action="" v-if="isLogged">
			<input type="text" v-model="serviceName">
			<textarea v-model="serviceDescription"></textarea>
			
		</form>

		<div id="show-login-token">{{ lgnTok }}</div>
		<div id="show-logged-details">{{ loggedInfo }}</div>
	</div>
</template>

<script>
const axios = require('axios')
// const js_cookie = require('js-cookie')
export default{
	name: 'CPSVForm',
	// template: '#cpsv_form',
	data: function(){
		return {
			baseAddress: 'http://wiki.localhost/api.php',
			axiosInstance: axios.create({
				baseURL: this.baseAddress
			}),
			isLogged: false,
			userName: '',
			password: '',
			serviceName: '',
			serviceDescription: '',
			loggedInfo: '',
			availableFields: {},
			// loginToken: 's',
			lgnTok: 'Login Token used',
			
		}
	},
	methods: {
		isNotLoggedIn: function(){
			if(this.loginToken === ''){
				return true;
			}
			else{
				return false
			}
		},
		getLoginToken: function(){
			var _that = this
			// console.log('this ' + this.loginToken)
			axios.get(this.baseAddress + "?action=query&meta=tokens&type=login&format=json")
			.then(function(response){
				// console.log(response.headers.set-cookie)
				// console.log(response.data.query.tokens)
				_that.lgnTok = response.data.query.tokens.logintoken
				return response.data.query.tokens.logintoken
			})
			.catch(function(error){
				console.log(error)
				// console.log(response.data.query.tokens.logintoken)
				// return response.data.query.tokens.logintoken
			})
		},
		getLoginFields: function(){
			var _that = this
			axios.get(this.baseAddress + "?action=query&meta=authmanagerinfo&amirequestsfor=login&format=json")
			.then(function(response){
				_that.availableFields = response.data.query.authmanagerinfo.requests[0].fields
				// console.log(response.data.query.authmanagerinfo.requests[0].fields)
				return true
			})
			.catch(function(error){
				console.log(error)
			})

		},
		login: function(){
			var _that=this
			axios({
				method: 'post',
				url: this.baseAddress,
				headers: {
					'Access-Control-Allow-Credentials': true,
					'Content-Type': 'application/x-www-form-urlencoded',
				},
				// data: 'action=clientlogin&lgmame=mandravinus&lgpassword=' + _that.password + '&lgtoken=' + this.lgnTok + '&format=json&loginreturnurl=https%3A%2F%2Fbit.ly%2F2zxmFw5'
				data: 'action=clientlogin&canauthenticatenow=true&authAction=clientlogin&username=' + _that.userName + '&password=' + _that.password + '&maxlag=0&loginrequests=&loginreturnurl=http%3A%2F%2Flocalhost%3A8080&logintoken=' + encodeURIComponent(this.lgnTok) + '&format=json&origin=*&required=primary-required&id=' + encodeURIComponent('MediaWiki\\Auth\\PasswordAuthenticationRequest') + '&provider=Password-based-authentication&account=',
				// data: {
				// 	action: 'clientlogin',
				// 	maxlag: '0',
				// 	loginrequests: '',
				// 	loginreturnurl: 'http%3A%2F%2Flocalhost%3A8080',
				// 	logintoken: encodeURIComponent(_that.lgnTok),
				// 	username: _that.userName,
				// 	password: _that.password,
				// 	format: 'json',
				// 	origin: '*'
				// },
				withCredentials: 'true'
			})
			.then(function(response){
				// console.log(that.baseAddress)
				// console.log('token_ ' + _that.lgnTok)
				// _that.lgnTok = response.data
				// console.log(response)
				// console.log('from error')
			})
			.catch(function(error){
				console.log(error)
			})

			// return response.data.query.tokens.logintoken
			// axios.post(this.baseAddress + '?action=login&lgname=mandravinus&lgpassword=halvstone96&lgtoken=' + this.loginToken + '&format=json&origin=http://localhost:8080', {
		}
	},
	created: function(){
		// this.loginToken = this.getLoginToken()
			axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded'
			axios.defaults.headers.post['Access-Control-Allow-Credentials'] = true
			axios.defaults.headers.post['Access-Control-Allow-Origin'] = '*'
			axios.defaults.headers.post['xsrfCookieName'] = 'XSRF-TOKEN'
			axios.defaults.withCredentials = true
			console.log(axios.defaults)
	},
	// computed: {
	// 	loginToken: function(){
	// 		this.getLoginToken()
	// 	}
	// },
	mounted: function() {
		this.getLoginToken()
		this.getLoginFields()
	}
}
// Mandravinus@mandravinus_bot
// 0mgpjitt4svk5os4441tnktr731femms


 // q2l8256qbdicg9apbgdab0nc5onqmqkk
</script>