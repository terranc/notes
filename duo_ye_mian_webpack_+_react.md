# 多页面Webpack + React {#webpack-react}

`积累` `Webpack` `React`

<pre class="prettyprint linenums prettyprinted">

1.  `var path = require(&#039;path&#039;);`
2.  `var glob = require(&#039;glob&#039;);`
3.  `var webpack = require(&#039;webpack&#039;);`
4.  `varExtractTextPlugin= require(&#039;extract-text-webpack-plugin&#039;);`
5.  `varHtmlWebpackPlugin= require(&#039;html-webpack-plugin&#039;);`
6.  `varCleanPlugin= require(&#039;clean-webpack-plugin&#039;);`
7.  `varChunkManifestPlugin= require(&#039;chunk-manifest-webpack-plugin&#039;);`
8.  `varUglifyJsPlugin= webpack.optimize.UglifyJsPlugin;`
9.  `var autoprefixer = require(&#039;autoprefixer&#039;);`
10.  
11.  `process.env.NODE_ENV =&#039;development&#039;;`
12.  `const debug = process.env.NODE_ENV !==&#039;production&#039;;`
13.  `const ouput_dir = debug ?&#039;build&#039;:&#039;assets&#039;;`
14.  
15.  `var entries = getEntry(&#039;src/js/page/**/*.js&#039;,&#039;src/js/page/&#039;);`
16.  `var chunks =Object.keys(entries);`
17.  `entries.common =[&#039;jquery&#039;,&#039;console-polyfill&#039;];`
18.  `// console.log(entries);`
19.  `var config ={`
20.  ` entry: entries,`
21.  ` output:{`
22.  ` path: path.join(__dirname, ouput_dir),`
23.  ` publicPath: debug ?&#039;/&#039;:&#039;/&#039;,`
24.  ` filename: debug ?&#039;js/[name].js&#039;:&#039;js/[name]-[hash].js&#039;,`
25.  ` chunkFilename:&#039;js/[hash]/js/[name].js&#039;,`
26.  `// hotUpdateMainFilename: &quot;js/[hash]/update.json&quot;,`
27.  `// hotUpdateChunkFilename: &quot;js/[hash]/js/[name].update.js&quot;,`
28.  `},`
29.  ` resolve:{`
30.  `// root: &#039;E:/www/temp/webpack_test&#039;, //绝对路径`
31.  ` extensions:[&#039;&#039;,&#039;.js&#039;,&#039;.jsx&#039;]`
32.  `},`
33.  ` externals:{`
34.  `&#039;rect&#039;:&#039;React&#039;,`
35.  `// &#039;rect-dom&#039;: &#039;ReactDOM&#039;`
36.  `},`
37.  ` devtool:&quot;source-map&quot;,`
38.  ` postcss:[ autoprefixer ],`
39.  ` module:{`
40.  ` loaders:[//加载器`
41.  `{`
42.  ` test:/\.json$/,`
43.  ` loader:&quot;json-loader&quot;`
44.  `},{`
45.  ` test:/\.css$/,`
46.  ` loader:ExtractTextPlugin.extract(&#039;style-loader&#039;,&#039;css&#039;)`
47.  `},{`
48.  ` test:/\.js[x]?$/,`
49.  ` exclude:/node_modules/,`
50.  ` loader:&#039;react-hot!jsx?harmony&#039;`
51.  `},{`
52.  ` test:/\.less$/,`
53.  ` loader:ExtractTextPlugin.extract(&#039;style-loader&#039;,&#039;css!less&#039;)`
54.  `},{`
55.  ` test:/\.html$/,`
56.  ` loader:&quot;html&quot;`
57.  `},{`
58.  ` test:/\.(woff|woff2|ttf|eot|svg)(\?v=[0-9]\.[0-9]\.[0-9])?$/,`
59.  ` loader:&#039;file-loader?name=./fonts/[name].[ext]&#039;`
60.  `},{`
61.  ` test:/\.(png|jpe?g|gif)$/,`
62.  ` loader:&#039;url-loader?limit=8192&amp;name=./images/[name]-[hash].[ext]&#039;`
63.  `}`
64.  `]`
65.  `},`
66.  ` plugins:[`
67.  `new webpack.BannerPlugin(&#039;Lookfeel © hello@lookfeel.co&#039;),`
68.  ` debug ?function(){}:newCleanPlugin([ouput_dir],{`
69.  ` verbose:true,`
70.  ` dry:false`
71.  `}),`
72.  `new webpack.ProvidePlugin({//加载jq`
73.  ` $:&quot;jquery&quot;,`
74.  ` jQuery:&quot;jquery&quot;`
75.  `}),`
76.  `new webpack.optimize.DedupePlugin(),//打包的时候删除重复或者相似的文件`
77.  `new webpack.optimize.CommonsChunkPlugin({`
78.  ` name:&#039;common&#039;,// 将公共模块提取，生成名为`common`的chunk`
79.  ` chunks: chunks,`
80.  ` minChunks: chunks.length // 提取所有entry共同依赖的模块`
81.  `}),`
82.  `newExtractTextPlugin(&#039;css/[name].css&#039;),//单独使用link标签加载css并设置路径，相对于output配置中的publickPath`
83.  ` debug ?function(){}:newUglifyJsPlugin({//压缩代码`
84.  ` compress:{`
85.  ` warnings:false`
86.  `},`
87.  ` except:[&#039;><#039;,&#039;exports&#039;,&#039;require&#039;]//排除关键字`
88.  `}),`
89.  `new webpack.NoErrorsPlugin(),`
90.  ` debug ?function(){}:newChunkManifestPlugin()`
91.  `],`
92.  ` alias:{`
93.  
94.  `}`
95.  `};`
96.  `//使用webpack-dev-server，提高开发效率`
97.  `// config.devServer = {`
98.  `//     publicPath: config.output.publicPath,`
99.  `//     contentBase: &#039;./&#039; + ouput_dir,`
100.  `//     hot: true,`
101.  `//     port: 9090, //默认8080`
102.  `//     host: &#039;localhost&#039;,`
103.  `//     historyApiFallback: false,`
104.  `//     noInfo: false,`
105.  `//     stats: {`
106.  `//         colors: true // 用颜色标识`
107.  `//     }`
108.  `// };`
109.  
110.  
111.  `var pages =Object.keys(getEntry(&#039;src/view/**/*.html&#039;,&#039;src/view/&#039;));`
112.  `pages.forEach(function(pathname){`
113.  `var conf ={`
114.  ` filename:&#039;../&#039;+ ouput_dir +&#039;/&#039;+ pathname +&#039;.html&#039;,//生成的html存放路径，相对于path`
115.  ` template: path.join(__dirname,&#039;/src/view/&#039;+ pathname +&#039;.html&#039;),//html模板路径`
116.  ` inject:false,//js插入的位置，true/&#039;head&#039;/&#039;body&#039;/false`
117.  ` minify:{//压缩HTML文件`
118.  ` removeComments:true,//移除HTML中的注释`
119.  ` collapseWhitespace:false//删除空白符与换行符`
120.  `}`
121.  `};`
122.  `if(pathname in config.entry){`
123.  ` conf.inject =&#039;body&#039;;`
124.  ` conf.chunks =[&#039;client&#039;,&#039;common&#039;, pathname];`
125.  `// conf.hash = true;`
126.  `}`
127.  ` config.plugins.push(newHtmlWebpackPlugin(conf));`
128.  `});`
129.  
130.  `module.exports = config;`
131.  
132.  `function getEntry(globPath, pathDir){`
133.  `var files = glob.sync(globPath);`
134.  `var entries ={},`
135.  ` entry, dirname, basename, pathname, extname;`
136.  
137.  `for(var i =0; i &lt; files.length; i++){`
138.  ` entry = files[i];`
139.  ` dirname = path.dirname(entry);`
140.  ` extname = path.extname(entry);`
141.  ` basename = path.basename(entry, extname);`
142.  ` pathname = path.join(dirname, basename);`
143.  ` pathname = pathname.replace(/\\/gi,&quot;/&quot;);`
144.  ` pathname = pathDir ? pathname.replace(newRegExp(&#039;^&#039;+ pathDir),&#039;&#039;): pathname;`
145.  ` entries[pathname]=[&#039;./&#039;+ entry];`
146.  `}`
147.  `return entries;`
148.  `}`

</pre>