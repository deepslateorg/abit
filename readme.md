# Installation
[![Release](https://jitpack.io/v/org.deepslate/Repo.svg?style=flat-square)]
(https://jitpack.io/#org.deepslate/abit)

## Maven
### Add the JitPack repository to your build file
```
	<repositories>
		<repository>
		    <id>jitpack.io</id>
		    <url>https://jitpack.io</url>
		</repository>
	</repositories>
```
### Add the dependency
Replacing Tag with the desired version.
```
	<dependency>
	    <groupId>org.deepslate</groupId>
	    <artifactId>abit</artifactId>
	    <version>Tag</version>
	</dependency>
```
## Gradle
### Add the JitPack repository to your build file in your root build.gradle at the end of repositories:
```
	dependencyResolutionManagement {
		repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
		repositories {
			mavenCentral()
			maven { url 'https://jitpack.io' }
		}
	}
```
### Add the dependency
```
	dependencies {
	        implementation 'org.deepslate:abit:Tag'
	}
```

# Spec
### key:
```
    [NNNNNNNN]   ╠ unsigned byte
    [BBBBBBBB]   ╗
       ....      ╠ number of bytes used for the key string is N+1 (UTF-8 encoded string)
    [BBBBBBBB]   ╝
```

### null:
```
    [0000|0000]
```

### boolean:
```
    true:  [0001|0001]
    false: [0000|0001]
```

### integer:
```
    [0XXX|0010] 
    [NNNNNNNN]   ╗
       ....      ╠ number of bytes used for the integer is X+1 (2s compliment & little-endian)    ║
    [NNNNNNNN]   ╝
```

### blob:
```
    [00XX|0011] 
    [NNNNNNNN]   ╗
       ....      ╠ number of bytes used for the integer is X+1 (2s compliment & little-endian)
    [NNNNNNNN]   ╝
    [BBBBBBBB]   ╗
       ....      ╠ number of bytes used for the blob is N
    [BBBBBBBB]   ╝
```

### string:
```
    [00XX|0100] 
    [NNNNNNNN]   ╗
       ....      ╠ number of bytes used for the integer is X+1 (2s compliment & little-endian)
    [NNNNNNNN]   ╝
    [SSSSSSSS]   ╗
       ....      ╠ number of bytes used for the string is N (UTF-8 encoded string)
    [SSSSSSSS]   ╝
```

### array:
```
    [00XX|0101] 
    [NNNNNNNN]   ╗
       ....      ╠ number of bytes used for the integer is X+1 (2s compliment & little-endian)
    [NNNNNNNN]   ╝
    [AAAAAAAA]   ╗
       ....      ╠ number of bytes used for the array is N
    [AAAAAAAA]   ╝
```

### tree:
```
    [00XX|0110] 
    [NNNNNNNN]   ╗
       ....      ╠ number of bytes used for the integer is X+1 (2s compliment & little-endian)
    [NNNNNNNN]   ╝
    [TTTTTTTT]   ╗
       ....      ╠ number of bytes used for the tree is N
    [TTTTTTTT]   ╝
```

### tree syntax:
```
    [  key   ] [ object ] ... [  key   ] [ object ]
```

### array syntax:
```
    [ object ] ... [ object ]
```

### other syntax:
* An integer must be the minimum amount of bytes required to represent it.
* While an array can be any order, trees need to be ordered such that smaller keys are first, if keys are of equal length, treat it as a big-endian integer and put the smaller integer first.