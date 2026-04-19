// ═══════════════════════════════════════════════════════════════
// XENIA PORTAL — FIX PATCH v1
// ═══════════════════════════════════════════════════════════════
// Fixes saving of answers for:
//   • text gaps without id
//   • pick (multiple choice) groups without id
//   • True/False rows (none have id in the original)
//   • checkboxes, homework items, accordion state
// Also fixes the Firebase echo-detection flag bug.
//
// How to use: put this file next to your HTML file, then add
//   <script src="fix.js"></script>
// right before </body> in the HTML.
// ═══════════════════════════════════════════════════════════════

(function(){
  'use strict';

  function elementKey(el){
    if(el.id) return 'id:' + el.id;
    var path = [];
    var cur = el;
    while(cur && cur !== document.body){
      var parent = cur.parentElement;
      if(!parent) break;
      var sameTag = Array.prototype.filter.call(parent.children, function(c){
        return c.tagName === cur.tagName && c.className === cur.className;
      });
      var idx = sameTag.indexOf(cur);
      path.unshift(cur.tagName + '.' + (cur.className || '') + '[' + idx + ']');
      if(parent.id){ path.unshift('#' + parent.id); break; }
      cur = parent;
    }
    return 'p:' + path.join('>');
  }

  window.collectState = function(){
    var state = {};
    document.querySelectorAll(
      'input[type="text"], input[type="url"], input.gap, input.order-input, textarea'
    ).forEach(function(el){
      if(el.value !== undefined && el.value !== '') state[elementKey(el)] = el.value;
    });
    document.querySelectorAll('.pick').forEach(function(pick){
      var sel = pick.querySelector('.pick-opt.selected');
      if(sel){
        var opts = pick.querySelectorAll('.pick-opt');
        state[elementKey(pick)] = Array.prototype.indexOf.call(opts, sel);
      }
    });
    document.querySelectorAll('.tf-row').forEach(function(row){
      if(row.querySelector('.sel-t')) state[elementKey(row)] = 'T';
      else if(row.querySelector('.sel-f')) state[elementKey(row)] = 'F';
    });
    document.querySelectorAll('input[type="checkbox"]').forEach(function(cb){
      if(cb.checked) state[elementKey(cb)] = true;
    });
    document.querySelectorAll('.simple-hw-list').forEach(function(list){
      var listKey = elementKey(list);
      list.querySelectorAll('li').forEach(function(li, idx){
        var chk = li.querySelector('.hw-chk');
        if(chk && chk.classList.contains('done')) state[listKey + ':hw:' + idx] = true;
      });
    });
    document.querySelectorAll('.unit-item, .lesson-accordion, .global-notes-wrap').forEach(function(el){
      if(el.classList.contains('open')) state['open:' + elementKey(el)] = true;
    });
    return state;
  };

  window.applyState = function(state){
    if(!state) return;
    var focused = document.activeElement;
    document.querySelectorAll(
      'input[type="text"], input[type="url"], input.gap, input.order-input, textarea'
    ).forEach(function(el){
      if(el === focused) return;
      var k = elementKey(el);
      if(state[k] !== undefined){
        el.value = state[k];
        var oninput = el.getAttribute('oninput') || '';
        var m = oninput.match(/countWords\(this,\s*['"]([^'"]+)['"]\)/);
        if(m){
          var wcEl = document.getElementById(m[1]);
          if(wcEl) wcEl.textContent = (el.value.trim().split(/\s+/).filter(Boolean).length) + ' words';
        }
      }
    });
    document.querySelectorAll('.pick').forEach(function(pick){
      var k = elementKey(pick);
      var idx = state[k];
      if(idx !== undefined){
        pick.querySelectorAll('.pick-opt').forEach(function(o){ o.classList.remove('selected'); });
        var opts = pick.querySelectorAll('.pick-opt');
        if(opts[idx]) opts[idx].classList.add('selected');
      }
    });
    document.querySelectorAll('.tf-row').forEach(function(row){
      var k = elementKey(row);
      var val = state[k];
      row.querySelectorAll('.tf-btn').forEach(function(b){ b.classList.remove('sel-t','sel-f'); });
      var btns = row.querySelectorAll('.tf-btn');
      if(val === 'T' && btns[0]) btns[0].classList.add('sel-t');
      else if(val === 'F' && btns[1]) btns[1].classList.add('sel-f');
    });
    document.querySelectorAll('input[type="checkbox"]').forEach(function(cb){
      cb.checked = state[elementKey(cb)] === true;
    });
    document.querySelectorAll('.simple-hw-list').forEach(function(list){
      var listKey = elementKey(list);
      list.querySelectorAll('li').forEach(function(li, idx){
        var chk = li.querySelector('.hw-chk');
        var txt = li.querySelector('.hw-txt');
        if(chk && state[listKey + ':hw:' + idx] === true){
          chk.classList.add('done'); chk.textContent = '\u2713';
          if(txt) txt.classList.add('done');
        }
      });
    });
    document.querySelectorAll('.unit-item, .lesson-accordion, .global-notes-wrap').forEach(function(el){
      var k = elementKey(el);
      if(state['open:' + k] === true) el.classList.add('open');
    });
  };

  var _saveTimer = null;
  window.saveAll = function(){
    clearTimeout(_saveTimer);
    _saveTimer = setTimeout(function(){
      var state = window.collectState();
      try { localStorage.setItem('xenia_portal_v1', JSON.stringify(state)); } catch(e){}
      if(window._dbRef){
        window._lastLocalSave = Date.now();
        window._dbRef.set(state)
          .then(function(){ if(window.showSynced) window.showSynced('\u2601\ufe0f Saved'); })
          .catch(function(){ if(window.showSynced) window.showSynced('\u26a0\ufe0f Offline'); });
      } else {
        if(window.showSynced) window.showSynced('\ud83d\udcbe Saved locally');
      }
    }, 600);
  };

  function patchFirebaseListener(){
    if(!window._dbRef){ setTimeout(patchFirebaseListener, 300); return; }
    try { window._dbRef.off('value'); } catch(e){}
    window._dbRef.on('value', function(snap){
      var s = snap.val();
      if(!s) return;
      if(window._lastLocalSave && (Date.now() - window._lastLocalSave) < 2000) return;
      window.applyState(s);
      if(window.showSynced) window.showSynced('\ud83d\udd04 Synced');
    });
    console.log('[Xenia patch] Firebase listener re-bound.');
  }

  function init(){
    patchFirebaseListener();
    setTimeout(function(){
      try {
        var raw = localStorage.getItem('xenia_portal_v1');
        if(raw) window.applyState(JSON.parse(raw));
      } catch(e){ console.error('[Xenia patch] reload failed', e); }
    }, 500);
    console.log('[Xenia patch] Loaded. collectState/applyState/saveAll overridden.');
  }

  if(document.readyState === 'loading'){
    document.addEventListener('DOMContentLoaded', init);
  } else {
    init();
  }
})();
