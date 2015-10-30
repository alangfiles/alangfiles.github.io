---
layout: post
title:  "Golfing down a Kata"
date:   2015-08-07 10:00:00
categories: codegolf kata
---


#Golfing down the code kata for Arabic Numeral to Roman Numeral conversion.
*Note: all character counts disregard whitespace, and I just explode the example code for readability :)
The basic idea:
{% highlight %}
for each roman numeral value:
  if that value is less than the input number:
    output the corresponding roman numeral, and 
    recursively call the function with the difference between roman value and input number as the new input number
{% endhighlight %}
    
##First Attempt (248 characters)
In which I try to solve the problem somewhat rationally.
{% highlight javascript %}
function toRoman(num){
    if(num<=0){return '';} //return condition for recursive call
  var map = [1000,'M',900,'CM',100,'C',90,'XC',50,'L',40,'XL',10,'X',9,'IX',5,'V',4,'IV',1,'I'];
  var retstring = '';
  for(var i = 0; i < map.length; i+=2){
    if(num-map[i] >= 0){
      num = num-map[i];
      return map[i+1]+toRoman(num);
    }
  }
}
{% endhighlight %}

##Second Attempt
In which I remove some variables, and add 'temp' (which will make it into even the shortest version)
{% highlight javascript %}
function toRoman(num){
  if(num<=0)return '';
  map=[1000,'M',900,'CM',100,'C',90,'XC',50,'L',40,'XL',10,'X',9,'IX',5,'V',4,'IV',1,'I'];
  for(i=0;i<22;i+=2){
    temp=num-map[i];
    if(temp>=0){
      num=temp;
      return map[i+1]+toRoman(num);
    }
  }
}
{% endhighlight %}

##Third Attempt
In which I make most of the variable names meaningless, but leave the function name (for now...)
{% highlight javascript %}
function toRoman(n){
  if(n<=0)return '';
  m=[1000,'M',900,'CM',100,'C',90,'XC',50,'L',40,'XL',10,'X',9,'IX',5,'V',4,'IV',1,'I'];
  for(i=0;i<22;i+=2){
    t=n-m[i];
    if(t>=0){
      n=t;
      return m[i+1]+toRoman(n);
    }
  }
}
{% endhighlight %}

In which I rearrange the logic, 
{% highlight javascript %}
function toRoman(n){
  m=[1000,'M',900,'CM',100,'C',90,'XC',50,'L',40,'XL',10,'X',9,'IX',5,'V',4,'IV',1,'I'];
  for(i=0;i<22;i+=2){
    if((t=n-m[i])>=0)break  //it'll break when we've found a number that can be subtracted from our initial 'n' (aka, a number we want to replace with a numeral)
  }
  return n<1?'':m[i+1]+toRoman(t)
}
{% endhighlight %}

In which I move the map of rules into 2 seperate arrays with the same index.
{% highlight javascript %}
function toRoman(n){
  for(i in a=[1000,900,100,90,50,40,10,9,5,4,1]){
    if((t=n-a[i])>=0)break
  }
  return n<1?'':"M,CM,C,XC,L,XL,X,IX,V,IV,I".split(',')[i]+toRoman(t)
}
{% endhighlight %}

In which I remove the function name, rip 'toRoman'
{% highlight javascript %}
function r(n){
  for(i in a=[1000,900,100,90,50,40,10,9,5,4,1])
    if((t=n-a[i])>=0)break;
  return n<1?'':"M,CM,C,XC,L,XL,X,IX,V,IV,I".split(',')[i]+r(t)
}
{% endhighlight %}

In which I split the roman numeral array on 0 instead of a char (saving 2 chars)
{% highlight javascript %}
function r(n){
  for(i in a=[1000,900,100,90,50,40,10,9,5,4,1])
    if((t=n-a[i])>=0)
      return "M0CM0C0XC0L0XL0X0IX0V0IV0I".split(0)[i]+r(t);
  return ''
}
{% endhighlight %}

## 141 chars
In which I found out 'for .. in' doesn't guarentee order, so I switch to a standard for loop
{% highlight javascript %}
r=function(n){
  for(i=11;i;)
    if((t=n-[1,4,5,9,10,40,50,90,100,900,1000][--i])>=0)
      return "I0IV0V0IX0X0XL0L0XC0C0CM0M".split(0)[i]+r(t);
  return ''
}
{% endhighlight %}

##141 chars
In which I have the idea to combine the roman array and arabic array into one, and split on 'digit' and 'non-digit'
Unfortunately, this saves no space.
{% highlight javascript %}
r=function(n){
  for(i=11;i;)
    if((t=n-(a="I1IV4V5IX9X10XL40L50XC90C100CM900M1000").split(/\D+/)[i--])>=0)
      return a.split(/\d+/)[i]+r(t);
  return ''
}
{% endhighlight %}

## None of the previous solutions are valid.
I forgot 2 roman numerals: 'D' (500), and 'CD' (400), so gotta add it, and the space savings from the single data array finally show up!
####154 old way
{% highlight javascript %}
r=function(n){
  for(i=13;i;)
    if((t=n-[1,4,5,9,10,40,50,90,100,400,500,900,1000][--i])>=0)
      return "I0IV0V0IX0X0XL0L0XC0C0CD0D0CM0M".split(0)[i]+r(t);
  return ''
}
{% endhighlight %}
####150 new way! 
{% highlight javascript %}
r=function(n){
  for(i=13,a="I1IV4V5IX9X10XL40L50XC90C100CD400D500CM900M1000";i;)
    if((t=n-a.split(/\D+/)[i--])>=0)
      return a.split(/\d+/)[i]+r(t);
  return ''
}
{% endhighlight %}

##147 chars
In which we make a single return!
{% highlight javascript %}
r=function(n){
  for(i=13,a="I1IV4V5IX9X10XL40L50XC90C100CD400D500CM900M1000";i;)
    if((t=n-a.split(/\D+/)[i--])>=0)
      return a.split(/\d+/)[i]+(t?r(t):'')
  }
{% endhighlight %}

##139 Chars
time to use ECMA6 arrow notation to make it tweetable:
{% highlight javascript %}
r=n=>{
  for(i=13,a="I1IV4V5IX9X10XL40L50XC90C100CD400D500CM900M1000";i;)
    if((t=n-a.split(/\D+/)[i--])>=0)
      return a.split(/\d+/)[i]+(t?r(t):'')
}
{% endhighlight %}

#125 chars
Golfed down with help, removed the for loop, and call the function recursively for that. using ecma6 default parameters
note: now the function is one time use, since `i` set outside of it.
{% highlight javascript %}
i=13;
r=(n,a="I1IV4V5IX9X10XL40L50XC90C100CD400D500CM900M1000",t=n-a.split(/\D+/)[i])=>
  i--?t>=0?a.split(/\d+/)[i]+r(t):r(n):''
{% endhighlight %}
#Final one liner:
{% highlight javascript %}
i=13;r=(n,a="I1IV4V5IX9X10XL40L50XC90C100CD400D500CM900M1000",t=n-a.split(/\D+/)[i])=>i--?t>=0?a.split(/\d+/)[i]+r(t):r(n):''
{% endhighlight %}