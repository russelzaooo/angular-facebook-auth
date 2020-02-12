# Autenticação com Facebook usando Angular 8

Este exemplo requer um conhecimento básico de Angular e Javascript.
Também é necessário que o desenvolverdor já tenha em mãos o ID do aplicativo do Facebook.

## 1 - Instalar a biblioteca angularx-social-login
~~~
npm install --save angularx-social-login
~~~

## 2 - Configurar o Provider do Facebook

Abra o arquivo **app.module.ts**

~~~javascript
...
const config = new AuthServiceConfig([
  {
    id: FacebookLoginProvider.PROVIDER_ID,
    provider: new FacebookLoginProvider('<FACEBOOK_APP_ID>')
  }
]);

export function provideConfig() {
  return config;
}
...
~~~

OBS.: O **<FACEBOOK_APP_ID>** deverá ser alterado pelo APP ID do aplicativo que você criou no Facebook.

Ainda no arquivo **app.module.ts**, adicione o seguinte array de Providers dentro do NgModule.

~~~javascript
...
 providers: [
   {
     provide: AuthServiceConfig,
     useFactory: provideConfig
   }
 ] 
 ...
~~~

## 3 - Declarando os métodos de Login e Logout

Abra o arquivo *app.component.ts*.
Dentro da classe, criar as variáveis de autenticação:

~~~javascript
user: SocialUser;
loggedIn: boolean;
~~~

Declarar o AuthService dentro do construtor:

~~~javascript
constructor(private authService: AuthService) { }
~~~

Ao iniciar o component, verificamos se o usuário já está logado:

~~~javascript
ngOnInit() {
  if (localStorage.fbToken) {
    this.loggedIn = true;
  }
  this.authService.authState.subscribe((user) => {
    this.user = user;
    this.loggedIn = (user != null);
    console.log(this.user);
  });
}
~~~  

Por fim os métodos de Login e Logout:

~~~javascript
signInWithFB(): void {
  this.authService.signIn(FacebookLoginProvider.PROVIDER_ID);
}

signOut(): void {
  this.authService.signOut();
}
~~~

## 4 - Layout (Extra)

Neste exemplo eu fiz uma tela que, caso o usuário esteja logado, exibe as informações do usuário logado que o Facebook retorna.
