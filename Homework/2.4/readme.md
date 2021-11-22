<body>
1.  $ git log|grep 'commit aefea'<br>
>>  commit aefead2207ef7e2aa5dc81a34aedf0cad4c32545<br><br>


2.  git show 85024d3<br>
>>  commit 85024d3100126de36331c6982bfaac02cdab9e76 (tag: v0.12.23)<br>
<b>tag: v0.12.23</b>
<br><br>

3.  git rev-parse b8d720^@ <br>
>>  56cd7859e05c36c06b56d013b55a252d0bb7e158<br>
>>  9ea88f22fc6269854151c571162c5bcf958bee2b<br><br>

4. $ git log --pretty=format:"%H %s" v0.12.23..v0.12.24 <br>
>>  33ff1c03bb960b332be3af2e333462dde88b279e v0.12.24 <br>
>>  b14b74c4939dcab573326f4e3ee2a62e23e12f89 [Website] vmc provider links <br>
>>  3f235065b9347a758efadc92295b540ee0a5e26e Update CHANGELOG.md <br>
>>  6ae64e247b332925b872447e9ce869657281c2bf registry: Fix panic when server is unreachable<br>
>>  5c619ca1baf2e21a155fcdb4c264cc9e24a2a353 website: Remove links to the getting started guide's old location<br>
>>  06275647e2b53d97d4f0a19a0fec11f6d69820b5 Update CHANGELOG.md<br>
>>  d5f9411f5108260320064349b757f55c09bc4b80 command: Fix bug when using terraform login on Windows<br>
>>  4b6d06cc5dcb78af637bbb19c198faff37a066ed Update CHANGELOG.md<br>
>>  dd01a35078f040ca984cdd349f18d0b67e486c35 Update CHANGELOG.md<br>
>>  225466bc3e5f35baa5d07197bbc079345b77525e Cleanup after v0.12.23 release<br><br>

5. $ git log --pretty=format:"%h,%ad,%s" -S "func providerSource"
>>  5af1e6234,Tue Apr 21 16:28:59 2020 -0700,main: Honor explicit provider_installation CLI config when present<br>
>>  <b>8c928e835,Thu Apr 2 18:04:39 2020 -0700,main: Consult local directories as potential mirrors of providers</b><br><br>

6.  $ git log --pretty=oneline -L :globalPluginDirs:plugins.go<br>
>>  78b12205587fe839f10d946ea3fdc06719decb05 Remove config.go and update things using its aliases<br>
>>  52dbf94834cb970b510f2fba853a5b49ad9b1a46 keep .terraform.d/plugins for discovery<br>
>>  41ab0aef7a0fe030e84018973a64135b11abcd70 Add missing OS_ARCH dir to global plugin paths<br>
>>  66ebff90cdfaa6938f26f908c7ebad8d547fea17 move some more plugin search path logic to command<br>
>>  8364383c359a6b738a436d1b7745ccdce178df47 Push plugin discovery down into command package<br><br>


7.  $ git log -S"func synchronizedWriters(" --pretty=format:'%H %an %s'<br  >
>> bdfea50cc85161dea41be0fe3381fd98731ff786 <b>James Bardin remove unused</b> <br>
>> 5ac311e2a91e381e2f52234668b49ba670aa0fe5 <b>Martin Atkins</b> main: synchronize writes to VT100-faker on Windows<br><br>


