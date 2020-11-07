# QuickStart

In almost all environments, you can obtain the currently executing user via the following call:

```java
Subject currentUser = SecurityUtils.getSubject();
```

If you want to make things available to the user during their current session with the application, you can get their session,it does not require an HTTP environment!:

```java
Session session = currentUser.getSession();
session.setAttribute( "someKey", "aValue" );
```

that is, until they log in at least once. So, let’s do that:

```java
if ( !currentUser.isAuthenticated() ) {
    //collect user principals and credentials in a gui specific manner
    //such as username/password html form, X509 certificate, OpenID, etc.
    //We'll use the username/password example here since it is the most common.
    //(do you know what movie this is from? ;)
    UsernamePasswordToken token = new UsernamePasswordToken("lonestarr", "vespa");
    //this is all you have to do to support 'remember me' (no config - built in!):
    token.setRememberMe(true);
    currentUser.login(token);
}
```

But what if their login attempt fails? You can catch all sorts of specific exceptions that tell you exactly what happened and allows you to handle and react accordingly:

```java
try {
    currentUser.login( token );
    //if no exception, that's it, we're done!
} catch ( UnknownAccountException uae ) {
    //username wasn't in the system, show them an error message?
} catch ( IncorrectCredentialsException ice ) {
    //password didn't match, try again?
} catch ( LockedAccountException lae ) {
    //account for that username is locked - can't login.  Show them a message?
}
    ... more types exceptions to check if you want ...
} catch ( AuthenticationException ae ) {
    //unexpected condition - error?
}
```

so by now, we have a logged in user. What else can we do?

Let’s say who they are:

```java
//print their identifying principal (in this case, a username): 
log.info( "User [" + currentUser.getPrincipal() + "] logged in successfully." );
```

We can also test to see if they have specific role or not:

```java
if ( currentUser.hasRole( "schwartz" ) ) {
    log.info("May the Schwartz be with you!" );
} else {
    log.info( "Hello, mere mortal." );
}
```

We can also see if they have a permission to act on a certain type of entity:

```java
if ( currentUser.isPermitted( "lightsaber:weild" ) ) {
    log.info("You may use a lightsaber ring.  Use it wisely.");
} else {
    log.info("Sorry, lightsaber rings are for schwartz masters only.");
}
```

Also, we can perform an extremely powerful *instance-level* permission check - the ability to see if the user has the ability to access a specific instance of a type:

```java
if ( currentUser.isPermitted( "winnebago:drive:eagle5" ) ) {
    log.info("You are permitted to 'drive' the 'winnebago' with license plate (id) 'eagle5'.  " +
                "Here are the keys - have fun!");
} else {
    log.info("Sorry, you aren't allowed to drive the 'eagle5' winnebago!");
}
```

when the user is done using the application, they can log out:

```java
currentUser.logout(); //removes all identifying information and invalidates their session too.
```

# 授权

| 权限名 | 权限范围                   |
| ------ | -------------------------- |
| anon   | 无需认证                   |
| authc  | 所有访问都要认证           |
| user   | 要有记住我才能访问         |
| role   | 拥有某个角色才能访问       |
| perms  | 拥有某个资源的权限才能访问 |

