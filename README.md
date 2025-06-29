# htaccess
Блокировка ботов и парсеров в .htaccess по USER_AGENT и REFERER (для разных CMC и для VPS).

В файле .htaccess пропишите такую блокировку от известных по USER_AGENT:

RewriteCond %{HTTP_USER_AGENT} (ClaudeBot|github|AdsTxtCrawlerTP|aiHitBot|AlphaBot|archive\.org_bot|Audit|AwarioBot|axios|BackupLand|Baiduspider|Barkrowler|Birubot|Bitrix|BLEXBot|Butterfly|CamontSpider|coccoc|coccocbot-web|Crawler|curl|CyotekWebCopy|DataForSeoBot|Discordbot|DnyzBot|DotBot|DuckDuckGo-Favicons-Bot|ev-crawler|EventMachine|Exabot|ExtLinksBot|Ezooms|FairShare|FlipboardProxy|GeedoBot|Gigabot|Go-http-client|gold\ crawler|got|HeadlessChrome|HostTracker|HTTrack|ia_archiver|Iframely|InAGist|INETDEX-BOT|intelx\.io_bot|InternetMeasurement|InternetSeer|Java|JS-Kit|keys-so-bot|kmSearchBot|Konturbot|Ktor|larbin|LetsearchBot|Library|Linespider|Linguee|LinkedInBot|LinkExchanger|LinkpadBot|ltx71|lwp-trivial|Mail\.RU_Bot|Mail\.RU_Bot/Fast|MauiBot|MCN|meanpathbot|Mediatoolkitbot|MegaIndex\.ru|METASpider|MetaURI|MixrankBot|MJ12bot|MLBot|Moreover|Nekstbot|NetcraftSurveyAgent|netEstate|Nigma\.ru|NING|NjuiceBot|None|Nutch|okhttp|page-audit|PaperLiBot|PetalBot|PHP/|PingAdmin\.Ru|PixelTools|PostRank|PR-CY\.RU|ptd-crawler|Purebot|PycURL|Python|python-httpx|python-requests|Python-urllib|QuerySeekerSpider|ReactorNetty|RockMelt|rogerbot|SafeDNSBot|Scooper|Scrapy|Screaming|ScribdReader|SeekportBot|SemrushBot|SEO|SEOkicks|SeopultContentAnalyzer|serpstatbot|SeznamBot|SiteAnalyzerbot|SiteBot|SiteCheckerBot|SiteExplorer|Slurp|SMTBot|SolomonoBot|Soup|spbot|Spider|SputnikBot|statdom\.ru|strawberryj|suggybot|Summify|SurdotlyBot|SurveyBot|SWeb|ttCrawler|TurnitinBot|TweetedTimes|TweetmemeBot|UnwindFetchor|Voyager|WebDataStats|WellKnownBot|Wget|WordPress|XenForo|Yeti|YottosBot|Zoombot|ZoominfoBot|Zeus|^\s*$) [NC]
RewriteRule ^ - [F,L]
Только проверьте список, может кто-то не хочет блокировать, например, таких ботов: ClaudeBot (ИИ по генерации текстов), WordPress (может нужен бот, для скриптов сайтов на Word Press) и т.п.

А также вот такую блокировку по REFERER:

RewriteCond %{HTTP_REFERER} (seohub\.by|adguard\.com|startpage\.com|top-page\.ru|petalsearch\.com|neuronwriter\.com|telegra\.ph|baidu\.com|brief\.ly|s\.zlnav\.com|italstroy\.ru|iframe-toloka\.com|mobilesearchers\.com|coolakov\.ru|newsearchers\.com|tmweb\.ru|weebly\.com|calipso\.by|maxask\.com|presearch\.com|bitrix24\.ru|seranking\.com|xranks\.com|url-opener\.com|relevantus\.org|rush-analytics\.ru|api\.pfbaza\.website|arsenkin\.ru|tools\.pixelplus\.ru|m\.gsearch\.co|Ask\.com|readmehouse\.ru|factorial-eval|bitrix24\.by|alohafind\.com|planfix\.ru|amocrm\.ru|localmedia\.by|dynamitedojo\.com|topvisor\.com) [NC]
RewriteRule ^ - [F,L]
Блокировка всех HEAD запросов (по ним приходят только боты, скрипты и т.п., в исключение добавлен Googlebot, т.к. он иногда заходит таким методом):

RewriteCond %{REQUEST_METHOD} ^HEAD$
RewriteCond %{HTTP_USER_AGENT} !Googlebot
RewriteRule ^ - [F,L]
Блокировка ботов, которые лезут взломать админки, тут аккуратнее будьте, для опытных пользователей, тут нужно прописать исключение для входа себя, в данном случае прописано исключение для подсети 109.126.0.0/16 (это работает так - если при заходе на сайт в адресе содержится, например, admin или wp-admin и т.п. - то запрос сразу блокируется и выдается ответ сервера 403 (доступ запрещен)):

RewriteCond %{REQUEST_URI} ^/(bitrix|wp-admin|administrator|user|admin|wp-[^/]+|wordpress|assets/js)|\.map$ [NC]
RewriteCond %{REMOTE_ADDR} !^109\.126\.[0-9]+\.[0-9]+$
RewriteRule .* - [F,L]
Во всех перечисленных блокировках, ботам будет выдаваться ответ сервера 403 (доступ запрещен). Некоторые парсеры, видя что их ботам отдается 403 вносят ваш сайт в исключение для посещений и через пару недель или месяцев перестают пытаться парсить ваш сайт.

Проверено на сайтах: https://sgm.by , https://remon.by
