ToffeeScript
============

ToffeeScript is a CoffeeScript dialect with Asynchronous Grammar

**Features**

1. Asynchronous everywhere
    * Condition: If, Switch
    * Loop: For In, For Of, While with guard `when`
    * Mathematics
    * Logical Operation
2. Auto Callback
3. Regexp Operator `=~` and matches `\~`, `\&`, `\0`~`\9`
4. High efficent code generated.
5. Sourcemap Supported.
    * Follow up to CoffeeScript 1.6.2 so far
6. Safety named-function supported.
	* Added in ToffeeScript 1.6.2-3

Installation
------------

    npm install toffee-script

Named Function
--------------
ToffeeScript support named function which is different from CoffeeScript.
if the function defined in the first level of code block and the function name haven't been used, then compile it as named function. see Code Examples section
It won't have compatible issue with CoffeeScript except one case

	# m never declared above, m must be local variable and assign to undefined
	m()
	m = ->
	m

in CoffeeScript will throw exception `undefined is not function`. Use m as constant undefined variable is rare case.

in ToffeeScript function m is hoisted, and will run function m() as Javascript does.

Code Examples
-------------
Left: ToffeeScript

Right: Generated JavaScript
### Basic

<table width=100%>
<tr>
	<td width=50% valign=top><pre>x, y = a! b
console.log x, y</pre></td>
	<td width=50% valign=top><pre>var x, y,
  _this = this;

a(b, function() {
  x = arguments[0], y = arguments[1];
  return console.log(x, y);
});</pre></td>
</tr>
</table>

with powerful CoffeeScript assignment `[...]`

<table width=100%>
<tr><td width=100%><pre>[@x, y...] = a! b
console.log @x, y</pre></td></tr>
<tr><td width=100%><pre>var y,
  _this = this,
  __slice = [].slice;

a(b, function() {
  _this.x = arguments[0], y = 2 &lt;= arguments.length ? __slice.call(arguments, 1) : [];
  return console.log(_this.x, y);
});</pre></td></tr>
</table>

### Condition

<table width=100%>
<tr>
	<td width=50% valign=top><pre>if i
  x = a!
else
  y = b!
console.log x, y</pre></td>
	<td width=50% valign=top><pre>var x, y,
  _this = this;

if (i) {
  a(function() {
    x = arguments[0];
    _$$_0();
  });
} else {
  b(function() {
    y = arguments[0];
    _$$_0();
  });
}

function _$$_0() {
  return console.log(x, y);
};</pre></td>
</tr>
</table>

Async in condition

<table width=100%>
<tr>
	<td width=50% valign=top><pre>if e = a!
  return cb(e)
foo()</pre></td>
	<td width=50% valign=top><pre>var e,
  _this = this;

a(function() {
  _$cb$_2(e = arguments[0]);
});
function _$cb$_2(_$$_0) {
  if (_$$_0) {
    return cb(e);
  } else {
    _$$_1();
  }
  function _$$_1() {
    return foo();
  };
};</pre></td>
</tr>
</table>

Async in condition with multi return

Async call always return first argument

<table width=100%>
<tr><td width=100%><pre>if e, data = fs.readFile! 'foo'
  return cb(e)
console.log data</pre></td></tr>
<tr><td width=100%><pre>var data, e,
  _this = this;

fs.readFile('foo', function() {
  _$cb$_2((e = arguments[0], data = arguments[1], e));
});
function _$cb$_2(_$$_0) {
  if (_$$_0) {
    return cb(e);
  } else {
    _$$_1();
  }
  function _$$_1() {
    return console.log(data);
  };
};</pre></td></tr>
</table>

### Loop
Support For In, For Of, While with guard `when`

<table width=100%>
<tr>
	<td width=50% valign=top><pre>xs = for i in [1..3] when i &gt; 2
  a!
  # return arguments[0] in default</pre></td>
	<td width=50% valign=top><pre>var i, xs, _$res$_1, _i,
  _this = this;

_$res$_1 = [];
i = _i = 1;
function _step() {
  i = ++_i;
  _body();
};
function _body() {
  if (_i &lt;= 3) {
    if (i &gt; 2) {
      a(function(_$$_2) {
        _step(_$res$_1.push(_$$_2));
      });
    } else {
      _step();
    }
  } else {
    _done();
  }
};
function _done() {
  _$cb$_0(_$res$_1);
};
_body();
function _$cb$_0() {
  return xs = arguments[0];
};</pre></td>
</tr>
</table>

### Mathematics

<table width=100%>
<tr>
	<td width=50% valign=top><pre>x = a! + b! * c!</pre></td>
	<td width=50% valign=top><pre>var x,
  _this = this;

a(function(_$$_1) {
  b(function(_$$_3) {
    c(function(_$$_4) {
      _$cb$_2(_$$_3 * _$$_4);
    });
  });
  function _$cb$_2(_$$_5) {
    _$cb$_0(_$$_1 + _$$_5);
  };
});
function _$cb$_0() {
  return x = arguments[0];
};</pre></td>
</tr>
</table>

### Object

<table width=100%>
<tr>
	<td width=50% valign=top><pre>A =
  a: a
  b: b!
  c: c</pre></td>
	<td width=50% valign=top><pre>var A, _$$_1,
  _this = this;

_$$_1 = a;
b(function(_$$_2) {
  _$cb$_0({
    a: _$$_1,
    b: _$$_2,
    c: c
  });
});
function _$cb$_0() {
  return A = arguments[0];
};</pre></td>
</tr>
</table>

### Logical
Support `||`, `&&`, `?`, `&&=`, `||=`, `?=`

<table width=100%>
<tr>
	<td width=50% valign=top><pre>x = a! || b!
console.log x</pre></td>
	<td width=50% valign=top><pre>var x,
  _this = this;

a(function(_$$_1) {
  if (_$$_1) {
    _$cb$_3(_$$_1);
  } else {
    b(function(_$$_2) {
      _$cb$_3(_$$_2);
    });
  }
});
function _$cb$_3(_$$_4) {
  _$cb$_0(_$$_4);
};
function _$cb$_0() {
  x = arguments[0];
  return console.log(x);
};</pre></td>
</tr>
</table>

### Auto Callback

<table width=100%>
<tr>
	<td width=50% valign=top><pre>a = (autocb) -&gt; return 3</pre></td>
	<td width=50% valign=top><pre>function a(autocb) {
  return autocb(3);
};</pre></td>
</tr>
</table>

Return Multiple Values

<table width=100%>
<tr>
	<td width=50% valign=top><pre>a = (autocb) -&gt; return null, 3</pre></td>
	<td width=50% valign=top><pre>function a(autocb) {
  return autocb(null, 3);
};</pre></td>
</tr>
</table>

### Regexp

<table width=100%>
<tr><td width=100%><pre>if a =~ b || b =~ c
  \~
  \&
  \0
  \9</pre></td></tr>
<tr><td width=100%><pre>var __matches;

if ((__matches = a.match(b)) || (__matches = b.match(c))) {
  __matches;
  __matches[0];
  __matches[0];
  __matches[9];
}</pre></td></tr>
</table>

### Named Function Supported


<table width=100%>
<tr>
	<td width=50% valign=top><pre>a = -&gt;
b = ->
null</pre></td>
	<td width=50% valign=top><pre>function a() {};

function b() {};

null;</pre></td>
</tr>
</table>

Those cases will be kept in non-named function

<table width=100%>
<tr>
	<td width=50% valign=top><pre>f = null
if a
  b = c -&gt;
d e = ->
f = ->
null</pre></td>
	<td width=50% valign=top><pre>var b, e, f;

f = null;

if (a) {
  b = c(function() {});
}

d(e = function() {});

f = function() {};

null;</pre></td>
</tr>
</table>
