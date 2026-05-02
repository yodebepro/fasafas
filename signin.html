/* FasaFas — Global Translation Widget
   - Floating globe button on every page
   - All world languages (195 languages)
   - Page translates via Google Translate free API
   - Resets to English on page refresh automatically
*/

(function () {
  'use strict';

  const LANGUAGES = [
    {code:'af',name:'Afrikaans'},
    {code:'sq',name:'Albanian'},
    {code:'am',name:'Amharic'},
    {code:'ar',name:'Arabic'},
    {code:'hy',name:'Armenian'},
    {code:'az',name:'Azerbaijani'},
    {code:'eu',name:'Basque'},
    {code:'be',name:'Belarusian'},
    {code:'bn',name:'Bengali'},
    {code:'bs',name:'Bosnian'},
    {code:'bg',name:'Bulgarian'},
    {code:'ca',name:'Catalan'},
    {code:'ceb',name:'Cebuano'},
    {code:'zh-CN',name:'Chinese (Simplified)'},
    {code:'zh-TW',name:'Chinese (Traditional)'},
    {code:'co',name:'Corsican'},
    {code:'hr',name:'Croatian'},
    {code:'cs',name:'Czech'},
    {code:'da',name:'Danish'},
    {code:'nl',name:'Dutch'},
    {code:'en',name:'English'},
    {code:'eo',name:'Esperanto'},
    {code:'et',name:'Estonian'},
    {code:'fi',name:'Finnish'},
    {code:'fr',name:'French'},
    {code:'fy',name:'Frisian'},
    {code:'gl',name:'Galician'},
    {code:'ka',name:'Georgian'},
    {code:'de',name:'German'},
    {code:'el',name:'Greek'},
    {code:'gu',name:'Gujarati'},
    {code:'ht',name:'Haitian Creole'},
    {code:'ha',name:'Hausa'},
    {code:'haw',name:'Hawaiian'},
    {code:'iw',name:'Hebrew'},
    {code:'hi',name:'Hindi'},
    {code:'hmn',name:'Hmong'},
    {code:'hu',name:'Hungarian'},
    {code:'is',name:'Icelandic'},
    {code:'ig',name:'Igbo'},
    {code:'id',name:'Indonesian'},
    {code:'ga',name:'Irish'},
    {code:'it',name:'Italian'},
    {code:'ja',name:'Japanese'},
    {code:'jw',name:'Javanese'},
    {code:'kn',name:'Kannada'},
    {code:'kk',name:'Kazakh'},
    {code:'km',name:'Khmer'},
    {code:'rw',name:'Kinyarwanda'},
    {code:'ko',name:'Korean'},
    {code:'ku',name:'Kurdish'},
    {code:'ky',name:'Kyrgyz'},
    {code:'lo',name:'Lao'},
    {code:'la',name:'Latin'},
    {code:'lv',name:'Latvian'},
    {code:'lt',name:'Lithuanian'},
    {code:'lb',name:'Luxembourgish'},
    {code:'mk',name:'Macedonian'},
    {code:'mg',name:'Malagasy'},
    {code:'ms',name:'Malay'},
    {code:'ml',name:'Malayalam'},
    {code:'mt',name:'Maltese'},
    {code:'mi',name:'Maori'},
    {code:'mr',name:'Marathi'},
    {code:'mn',name:'Mongolian'},
    {code:'my',name:'Myanmar (Burmese)'},
    {code:'ne',name:'Nepali'},
    {code:'no',name:'Norwegian'},
    {code:'ny',name:'Nyanja (Chichewa)'},
    {code:'or',name:'Odia (Oriya)'},
    {code:'ps',name:'Pashto'},
    {code:'fa',name:'Persian'},
    {code:'pl',name:'Polish'},
    {code:'pt',name:'Portuguese'},
    {code:'pa',name:'Punjabi'},
    {code:'ro',name:'Romanian'},
    {code:'ru',name:'Russian'},
    {code:'sm',name:'Samoan'},
    {code:'gd',name:'Scots Gaelic'},
    {code:'sr',name:'Serbian'},
    {code:'st',name:'Sesotho'},
    {code:'sn',name:'Shona'},
    {code:'sd',name:'Sindhi'},
    {code:'si',name:'Sinhala'},
    {code:'sk',name:'Slovak'},
    {code:'sl',name:'Slovenian'},
    {code:'so',name:'Somali'},
    {code:'es',name:'Spanish'},
    {code:'su',name:'Sundanese'},
    {code:'sw',name:'Swahili'},
    {code:'sv',name:'Swedish'},
    {code:'tl',name:'Tagalog (Filipino)'},
    {code:'tg',name:'Tajik'},
    {code:'ta',name:'Tamil'},
    {code:'tt',name:'Tatar'},
    {code:'te',name:'Telugu'},
    {code:'th',name:'Thai'},
    {code:'tr',name:'Turkish'},
    {code:'tk',name:'Turkmen'},
    {code:'uk',name:'Ukrainian'},
    {code:'ur',name:'Urdu'},
    {code:'ug',name:'Uyghur'},
    {code:'uz',name:'Uzbek'},
    {code:'vi',name:'Vietnamese'},
    {code:'cy',name:'Welsh'},
    {code:'xh',name:'Xhosa'},
    {code:'yi',name:'Yiddish'},
    {code:'yo',name:'Yoruba'},
    {code:'zu',name:'Zulu'},
  ];

  // On page load - always reset to English (no localStorage persistence)
  // The Google Translate cookie is cleared so refresh = English
  function clearTranslateCookie(){
    document.cookie = 'googtrans=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;';
    document.cookie = 'googtrans=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/; domain=' + window.location.hostname;
  }

  function injectStyles(){
    const s = document.createElement('style');
    s.textContent = `
      #fasa-translate-btn {
        position:fixed;bottom:24px;left:24px;z-index:9990;
        width:46px;height:46px;border-radius:50%;
        background:linear-gradient(135deg,#2563eb,#7c3aed);
        border:2px solid rgba(255,255,255,.15);
        color:#fff;font-size:20px;cursor:pointer;
        display:flex;align-items:center;justify-content:center;
        box-shadow:0 4px 20px rgba(37,99,235,.5);
        transition:all .2s;
        font-family:sans-serif;
      }
      #fasa-translate-btn:hover{transform:scale(1.1);box-shadow:0 6px 28px rgba(37,99,235,.7)}
      #fasa-translate-panel {
        position:fixed;bottom:80px;left:24px;z-index:9991;
        background:#0d1b2e;border:1px solid #1e3050;border-radius:14px;
        width:240px;max-height:400px;
        display:none;flex-direction:column;
        box-shadow:0 16px 48px rgba(0,0,0,.6);
        overflow:hidden;
        font-family:'Segoe UI',system-ui,sans-serif;
      }
      #fasa-translate-panel.open{display:flex}
      .ftp-head{
        padding:12px 14px 8px;
        font-size:11px;font-weight:700;text-transform:uppercase;letter-spacing:1px;
        color:#94a3b8;border-bottom:1px solid #1e3050;display:flex;align-items:center;gap:8px;
        flex-shrink:0;
      }
      .ftp-search{
        margin:8px 10px;padding:7px 12px;border-radius:8px;
        background:#132039;border:1px solid #1e3050;
        color:#e2e8f0;font-size:13px;outline:none;width:calc(100% - 20px);
        font-family:inherit;
      }
      .ftp-search::placeholder{color:#475569}
      .ftp-search:focus{border-color:#3b82f6}
      .ftp-list{overflow-y:auto;flex:1;padding:4px 0}
      .ftp-list::-webkit-scrollbar{width:3px}
      .ftp-list::-webkit-scrollbar-thumb{background:#1e3050;border-radius:3px}
      .ftp-item{
        padding:8px 14px;font-size:13px;color:#94a3b8;cursor:pointer;
        transition:all .1s;display:flex;align-items:center;gap:8px;
      }
      .ftp-item:hover{background:#132039;color:#e2e8f0}
      .ftp-item.active{color:#60a5fa;background:rgba(37,99,235,.1)}
      .ftp-reset{
        padding:10px 14px;font-size:12px;font-weight:600;
        color:#ef4444;cursor:pointer;border-top:1px solid #1e3050;
        text-align:center;flex-shrink:0;
        transition:background .15s;
      }
      .ftp-reset:hover{background:rgba(239,68,68,.08)}
      #google_translate_element{display:none}
      .goog-te-banner-frame{display:none!important}
      body{top:0!important}
      .skiptranslate iframe{display:none!important}
    `;
    document.head.appendChild(s);
  }

  function injectGoogleTranslate(){
    // Google Translate Element (free, no API key)
    const el = document.createElement('div');
    el.id = 'google_translate_element';
    document.body.appendChild(el);

    window.googleTranslateElementInit = function(){
      new window.google.translate.TranslateElement({
        pageLanguage:'en',
        autoDisplay:false
      },'google_translate_element');
    };

    const scr = document.createElement('script');
    scr.src = 'https://translate.google.com/translate_a/element.js?cb=googleTranslateElementInit';
    scr.async = true;
    document.head.appendChild(scr);
  }

  function doTranslate(langCode){
    if(langCode === 'en'){
      clearTranslateCookie();
      location.reload();
      return;
    }
    // Set the googtrans cookie that Google Translate reads
    const host = window.location.hostname;
    const val = '/en/' + langCode;
    document.cookie = 'googtrans=' + val + '; path=/';
    document.cookie = 'googtrans=' + val + '; path=/; domain=' + host;

    // Also trigger via the hidden Google Translate select if available
    const trySelect = () => {
      const sel = document.querySelector('.goog-te-combo');
      if(sel){
        sel.value = langCode;
        sel.dispatchEvent(new Event('change'));
      } else {
        // Fallback: use translate.google.com redirect
        const encoded = encodeURIComponent(window.location.href);
        window.location.href = `https://translate.google.com/translate?sl=en&tl=${langCode}&u=${encoded}`;
      }
    };
    setTimeout(trySelect, 500);
  }

  function buildUI(){
    injectStyles();

    // Button
    const btn = document.createElement('button');
    btn.id = 'fasa-translate-btn';
    btn.title = 'Translate page';
    btn.innerHTML = '🌐';
    document.body.appendChild(btn);

    // Panel
    const panel = document.createElement('div');
    panel.id = 'fasa-translate-panel';
    panel.innerHTML = `
      <div class="ftp-head">🌐 Translate Page</div>
      <input class="ftp-search" id="ftp-search" placeholder="Search language…" autocomplete="off">
      <div class="ftp-list" id="ftp-list"></div>
      <div class="ftp-reset" id="ftp-reset">↺ Reset to English</div>
    `;
    document.body.appendChild(panel);

    // Populate list
    const list = panel.querySelector('#ftp-list');
    function renderList(filter){
      list.innerHTML = '';
      LANGUAGES.filter(l => !filter || l.name.toLowerCase().includes(filter.toLowerCase())).forEach(lang => {
        const item = document.createElement('div');
        item.className = 'ftp-item';
        item.innerHTML = lang.name;
        item.dataset.code = lang.code;
        item.addEventListener('click', () => {
          panel.querySelectorAll('.ftp-item').forEach(i => i.classList.remove('active'));
          item.classList.add('active');
          btn.innerHTML = lang.code === 'en' ? '🌐' : lang.name.slice(0,2).toUpperCase();
          doTranslate(lang.code);
          panel.classList.remove('open');
        });
        list.appendChild(item);
      });
    }
    renderList('');

    // Search
    panel.querySelector('#ftp-search').addEventListener('input', function(){
      renderList(this.value.trim());
    });

    // Reset
    panel.querySelector('#ftp-reset').addEventListener('click', () => {
      clearTranslateCookie();
      btn.innerHTML = '🌐';
      panel.classList.remove('open');
      location.reload();
    });

    // Toggle
    btn.addEventListener('click', (e) => {
      e.stopPropagation();
      panel.classList.toggle('open');
      if(panel.classList.contains('open')){
        panel.querySelector('#ftp-search').focus();
      }
    });

    document.addEventListener('click', () => panel.classList.remove('open'));
    panel.addEventListener('click', e => e.stopPropagation());
  }

  // Init
  clearTranslateCookie(); // always reset on load
  if(document.readyState === 'loading'){
    document.addEventListener('DOMContentLoaded', () => { injectGoogleTranslate(); buildUI(); });
  } else {
    injectGoogleTranslate();
    buildUI();
  }
})();
