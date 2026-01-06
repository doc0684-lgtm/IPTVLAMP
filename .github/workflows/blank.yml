(function () {
  'use strict';

  function _classCallCheck(instance, Constructor) {
    if (!(instance instanceof Constructor)) {
      throw new TypeError("Cannot call a class as a function");
    }
  }

  function _defineProperties(target, props) {
    for (var i = 0; i < props.length; i++) {
      var descriptor = props[i];
      descriptor.enumerable = descriptor.enumerable || false;
      descriptor.configurable = true;
      if ("value" in descriptor) descriptor.writable = true;
      Object.defineProperty(target, descriptor.key, descriptor);
    }
  }

  function _createClass(Constructor, protoProps, staticProps) {
    if (protoProps) _defineProperties(Constructor.prototype, protoProps);
    if (staticProps) _defineProperties(Constructor, staticProps);
    return Constructor;
  }

  function _defineProperty(obj, key, value) {
    if (key in obj) {
      Object.defineProperty(obj, key, {
        value: value,
        enumerable: true,
        configurable: true,
        writable: true
      });
    } else {
      obj[key] = value;
    }

    return obj;
  }

  function _unsupportedIterableToArray(o, minLen) {
    if (!o) return;
    if (typeof o === "string") return _arrayLikeToArray(o, minLen);
    var n = Object.prototype.toString.call(o).slice(8, -1);
    if (n === "Object" && o.constructor) n = o.constructor.name;
    if (n === "Map" || n === "Set") return Array.from(o);
    if (n === "Arguments" || /^(?:Ui|I)nt(?:8|16|32)(?:Clamped)?Array$/.test(n)) return _arrayLikeToArray(o, minLen);
  }

  function _arrayLikeToArray(arr, len) {
    if (len == null || len > arr.length) len = arr.length;

    for (var i = 0, arr2 = new Array(len); i < len; i++) arr2[i] = arr[i];

    return arr2;
  }

  function _createForOfIteratorHelper(o, allowArrayLike) {
    var it = typeof Symbol !== "undefined" && o[Symbol.iterator] || o["@@iterator"];

    if (!it) {
      if (Array.isArray(o) || (it = _unsupportedIterableToArray(o)) || allowArrayLike && o && typeof o.length === "number") {
        if (it) o = it;
        var i = 0;

        var F = function () {};

        return {
          s: F,
          n: function () {
            if (i >= o.length) return {
              done: true
            };
            return {
              done: false,
              value: o[i++]
            };
          },
          e: function (e) {
            throw e;
          },
          f: F
        };
      }

      throw new TypeError("Invalid attempt to iterate non-iterable instance.\nIn order to be iterable, non-array objects must have a [Symbol.iterator]() method.");
    }

    var normalCompletion = true,
        didErr = false,
        err;
    return {
      s: function () {
        it = it.call(o);
      },
      n: function () {
        var step = it.next();
        normalCompletion = step.done;
        return step;
      },
      e: function (e) {
        didErr = true;
        err = e;
      },
      f: function () {
        try {
          if (!normalCompletion && it.return != null) it.return();
        } finally {
          if (didErr) throw err;
        }
      }
    };
  }

  var Utils = /*#__PURE__*/function () {
    function Utils() {
      _classCallCheck(this, Utils);
    }

    _createClass(Utils, null, [{
      key: "clear",
      value: function clear(str) {
        return str.replace(/\&quot;/g, '"').replace(/\&#039;/g, "'").replace(/\&amp;/g, "&").replace(/\&.+?;/g, '');
      }
    }, {
      key: "isHD",
      value: function isHD(name) {
        var math = name.toLowerCase().match(' .hd$| .нd$| .hd | .нd | hd$| нd&| hd | нd ');
        return math ? math[0].trim() : '';
      }
    }, {
      key: "clearHDSD",
      value: function clearHDSD(name) {
        return name.replace(/ hd$| нd$| .hd$| .нd$/gi, '').replace(/ sd$/gi, '').replace(/ hd | нd | .hd | .нd /gi, ' ').replace(/ sd /gi, ' ');
      }
    }, {
      key: "clearMenuName",
      value: function clearMenuName(name) {
        return name.replace(/^\d+\. /gi, '').replace(/^\d+ /gi, '');
      }
    }, {
      key: "clearChannelName",
      value: function clearChannelName(name) {
        return this.clearHDSD(this.clear(name));
      }
    }, {
      key: "hasArchive",
      value: function hasArchive(channel) {
        if (channel.catchup) {
          var days = parseInt(channel.catchup.days);
          if (!isNaN(days) && days > 0) return days;
        }

        return 0;
      }
    }, {
      key: "canUseDB",
      value: function canUseDB() {
        return DB.db && Lampa.Storage.get('iptv_use_db', 'indexdb') == 'indexdb';
      }
    }]);

    return Utils;
  }();

  var favorites = [];

  var Favorites = /*#__PURE__*/function () {
    function Favorites() {
      _classCallCheck(this, Favorites);
    }

    _createClass(Favorites, null, [{
      key: "load",
      value: function load() {
        var _this = this;

        return new Promise(function (resolve, reject) {
          if (Utils.canUseDB()) {
            DB.getData('favorites').then(function (result) {
              favorites = result || [];
            })["finally"](resolve);
          } else {
            _this.nosuport();

            resolve();
          }
        });
      }
    }, {
      key: "nosuport",
      value: function nosuport() {
        favorites = Lampa.Storage.get('iptv_favorite_channels', '[]');
      }
    }, {
      key: "list",
      value: function list() {
        return favorites;
      }
    }, {
      key: "key",
      value: function key() {
        return Lampa.Storage.get('iptv_favotite_save', 'url');
      }
    }, {
      key: "find",
      value: function find(favorite) {
        var _this2 = this;

        return favorites.find(function (a) {
          return a[_this2.key()] == favorite[_this2.key()];
        });
      }
    }, {
      key: "remove",
      value: function remove(favorite) {
        var _this3 = this;

        return new Promise(function (resolve, reject) {
          var find = favorites.find(function (a) {
            return a[_this3.key()] == favorite[_this3.key()];
          });

          if (find) {
            if (Utils.canUseDB()) {
              DB.deleteData('favorites', favorite[_this3.key()]).then(function () {
                Lampa.Arrays.remove(favorites, find);
                resolve();
              })["catch"](reject);
            } else {
              Lampa.Arrays.remove(favorites, find);
              Lampa.Storage.set('iptv_favorite_channels', favorites);
              resolve();
            }
          } else reject();
        });
      }
    }, {
      key: "add",
      value: function add(favorite) {
        var _this4 = this;

        return new Promise(function (resolve, reject) {
          if (!favorites.find(function (a) {
            return a[_this4.key()] == favorite[_this4.key()];
          })) {
            Lampa.Arrays.extend(favorite, {
              view: 0,
              added: Date.now()
            });

            if (Utils.canUseDB()) {
              DB.addData('favorites', favorite[_this4.key()], favorite).then(function () {
                favorites.push(favorite);
                resolve();
              })["catch"](reject);
            } else {
              favorites.push(favorite);
              Lampa.Storage.set('iptv_favorite_channels', favorites);
              resolve();
            }
          } else reject();
        });
      }
    }, {
      key: "update",
      value: function update(favorite) {
        var _this5 = this;

        return new Promise(function (resolve, reject) {
          if (favorites.find(function (a) {
            return a[_this5.key()] == favorite[_this5.key()];
          })) {
            Lampa.Arrays.extend(favorite, {
              view: 0,
              added: Date.now()
            });
            if (Utils.canUseDB()) DB.updateData('favorites', favorite[_this5.key()], favorite).then(resolve)["catch"](reject);else {
              Lampa.Storage.set('iptv_favorite_channels', favorites);
              resolve();
            }
          } else reject();
        });
      }
    }, {
      key: "toggle",
      value: function toggle(favorite) {
        return this.find(favorite) ? this.remove(favorite) : this.add(favorite);
      }
    }]);

    return Favorites;
  }();

  var locked = [];

  var Locked = /*#__PURE__*/function () {
    function Locked() {
      _classCallCheck(this, Locked);
    }

    _createClass(Locked, null, [{
      key: "load",
      value: function load() {
        var _this = this;

        return new Promise(function (resolve, reject) {
          if (Utils.canUseDB()) {
            DB.getData('locked').then(function (result) {
              locked = result || [];
            })["finally"](resolve);
          } else {
            _this.nosuport();

            resolve();
          }
        });
      }
    }, {
      key: "nosuport",
      value: function nosuport() {
        locked = Lampa.Storage.get('iptv_locked_channels', '[]');
      }
    }, {
      key: "list",
      value: function list() {
        return locked;
      }
    }, {
      key: "find",
      value: function find(key) {
        return locked.find(function (a) {
          return a == key;
        });
      }
    }, {
      key: "format",
      value: function format(type, element) {
        return type == 'channel' ? 'channel:' + element[Lampa.Storage.get('iptv_favotite_save', 'url')] : type == 'group' ? 'group:' + element : 'other:' + element;
      }
    }, {
      key: "remove",
      value: function remove(key) {
        return new Promise(function (resolve, reject) {
          var find = locked.find(function (a) {
            return a == key;
          });

          if (find) {
            if (Utils.canUseDB()) {
              DB.deleteData('locked', key).then(function () {
                Lampa.Arrays.remove(locked, find);
                resolve();
              })["catch"](reject);
            } else {
              Lampa.Arrays.remove(locked, find);
              Lampa.Storage.set('iptv_locked_channels', locked);
              resolve();
            }
          } else reject();
        });
      }
    }, {
      key: "add",
      value: function add(key) {
        return new Promise(function (resolve, reject) {
          if (!locked.find(function (a) {
            return a == key;
          })) {
            if (Utils.canUseDB()) {
              DB.addData('locked', key, key).then(function () {
                locked.push(key);
                resolve();
              })["catch"](reject);
            } else {
              locked.push(key);
              Lampa.Storage.set('iptv_locked_channels', locked);
              resolve();
            }
          } else reject();
        });
      }
    }, {
      key: "update",
      value: function update(key) {
        return new Promise(function (resolve, reject) {
          if (locked.find(function (a) {
            return a == key;
          })) {
            if (Utils.canUseDB()) DB.updateData('locked', key, key).then(resolve)["catch"](reject);else {
              Lampa.Storage.set('iptv_locked_channels', locked);
              resolve();
            }
          } else reject();
        });
      }
    }, {
      key: "toggle",
      value: function toggle(key) {
        return this.find(key) ? this.remove(key) : this.add(key);
      }
    }]);

    return Locked;
  }();

  var DB = new Lampa.DB('cub_iptv', ['playlist', 'params', 'epg', 'favorites', 'other', 'epg_channels', 'locked'], 6);
  DB.logs = true;
  DB.openDatabase().then(function () {
    Favorites.load();
    Locked.load();
  })["catch"](function () {
    Favorites.nosuport();
    Locked.nosuport();
  });

  function fixParams(params_data) {
    var params = params_data || {};
    Lampa.Arrays.extend(params, {
      update: 'none',
      update_time: Date.now(),
      loading: 'cub'
    });
    return params;
  }

  var Params = /*#__PURE__*/function () {
    function Params() {
      _classCallCheck(this, Params);
    }

    _createClass(Params, null, [{
      key: "get",
      value: function get(id) {
        return new Promise(function (resolve) {
          if (Utils.canUseDB()) {
            DB.getDataAnyCase('params', id).then(function (params) {
              resolve(fixParams(params));
            });
          } else {
            resolve(fixParams(Lampa.Storage.get('iptv_playlist_params_' + id, '{}')));
          }
        });
      }
    }, {
      key: "set",
      value: function set(id, params) {
        if (Utils.canUseDB()) {
          return DB.rewriteData('params', id, fixParams(params));
        } else {
          return new Promise(function (resolve) {
            Lampa.Storage.set('iptv_playlist_params_' + id, fixParams(params));
            resolve();
          });
        }
      }
    }, {
      key: "value",
      value: function value(params, name) {
        return Lampa.Lang.translate('iptv_params_' + params[name]);
      }
    }]);

    return Params;
  }();

  function isValidPath(string) {
    var url;

    try {
      url = new URL(string);
    } catch (_) {
      return false;
    }

    return url.protocol === "http:" || url.protocol === "https:";
  }

  var Parser$1 = {};

  Parser$1.parse = function (content) {
    var playlist = {
      header: {},
      items: []
    };
    var lines = content.split('\n').map(parseLine);
    var firstLine = lines.find(function (l) {
      return l.index === 0;
    });
    if (!firstLine || !/^#EXTM3U/.test(firstLine.raw)) throw new Error('Playlist is not valid');
    playlist.header = parseHeader(firstLine);
    var i = 0;
    var items = {};

    var _iterator = _createForOfIteratorHelper(lines),
        _step;

    try {
      for (_iterator.s(); !(_step = _iterator.n()).done;) {
        var line = _step.value;
        if (line.index === 0) continue;
        var string = line.raw.toString().trim();

        if (string.startsWith('#EXTINF:')) {
          var EXTINF = string;
          items[i] = {
            name: EXTINF.getName(),
            tvg: {
              id: EXTINF.getAttribute('tvg-id'),
              name: EXTINF.getAttribute('tvg-name'),
              logo: EXTINF.getAttribute('tvg-logo'),
              url: EXTINF.getAttribute('tvg-url'),
              rec: EXTINF.getAttribute('tvg-rec')
            },
            group: {
              title: EXTINF.getAttribute('group-title')
            },
            http: {
              referrer: '',
              'user-agent': EXTINF.getAttribute('user-agent')
            },
            url: undefined,
            raw: line.raw,
            line: line.index + 1,
            catchup: {
              type: EXTINF.getAttribute('catchup'),
              days: EXTINF.getAttribute('catchup-days'),
              source: EXTINF.getAttribute('catchup-source')
            },
            timeshift: EXTINF.getAttribute('timeshift')
          };
        } else if (string.startsWith('#EXTVLCOPT:')) {
          if (!items[i]) continue;
          var EXTVLCOPT = string;
          items[i].http.referrer = EXTVLCOPT.getOption('http-referrer') || items[i].http.referrer;
          items[i].http['user-agent'] = EXTVLCOPT.getOption('http-user-agent') || items[i].http['user-agent'];
          items[i].raw += "\r\n".concat(line.raw);
        } else if (string.startsWith('#EXTGRP:')) {
          if (!items[i]) continue;
          var EXTGRP = string;
          items[i].group.title = EXTGRP.getValue() || items[i].group.title;
          items[i].raw += "\r\n".concat(line.raw);
        } else {
          if (!items[i]) continue;
          var url = string.getURL();
          var user_agent = string.getParameter('user-agent');
          var referrer = string.getParameter('referer');

          if (url && isValidPath(url)) {
            items[i].url = url;
            items[i].http['user-agent'] ='SmartTV';
            items[i].http.referrer = referrer || items[i].http.referrer;
            items[i].raw += "\r\n".concat(line.raw);
            i++;
          } else {
            if (!items[i]) continue;
            items[i].raw += "\r\n".concat(line.raw);
          }
        }
      }
    } catch (err) {
      _iterator.e(err);
    } finally {
      _iterator.f();
    }

    playlist.items = Object.values(items);
    return playlist;
  };

  function parseLine(line, index) {
    return {
      index: index,
      raw: line
    };
  }

  function parseHeader(line) {
    var supportedAttrs = ['x-tvg-url', 'url-tvg'];
    var attrs = {};

    for (var _i = 0, _supportedAttrs = supportedAttrs; _i < _supportedAttrs.length; _i++) {
      var attrName = _supportedAttrs[_i];
      var tvgUrl = line.raw.getAttribute(attrName);

      if (tvgUrl) {
        attrs[attrName] = tvgUrl;
      }
    }

    return {
      attrs: attrs,
      raw: line.raw
    };
  }

  String.prototype.getName = function () {
    var name = this.split(/[\r\n]+/).shift().split(',').pop();
    return name || '';
  };

  String.prototype.getAttribute = function (name) {
    var regex = new RegExp(name + '="(.*?)"', 'gi');
    var match = regex.exec(this);
    return match && match[1] ? match[1] : '';
  };

  String.prototype.getOption = function (name) {
    var regex = new RegExp(':' + name + '=(.*)', 'gi');
    var match = regex.exec(this);
    return match && match[1] && typeof match[1] === 'string' ? match[1].replace(/\"/g, '') : '';
  };

  String.prototype.getValue = function (name) {
    var regex = new RegExp(':(.*)', 'gi');
    var match = regex.exec(this);
    return match && match[1] && typeof match[1] === 'string' ? match[1].replace(/\"/g, '') : '';
  };

  String.prototype.getURL = function () {
    return this.split('|')[0] || '';
  };

  String.prototype.getParameter = function (name) {
    var params = this.replace(/^(.*)\|/, '');
    var regex = new RegExp(name + '=(\\w[^&]*)', 'gi');
    var match = regex.exec(params);
    return match && match[1] ? match[1] : '';
  };

  var Api = /*#__PURE__*/function () {
    function Api() {
      _classCallCheck(this, Api);
    }

    _createClass(Api, null, [{
      key: "get",
      value: function get(method, catch_error) {
        var _this = this;

        return new Promise(function (resolve, reject) {
          var account = Lampa.Storage.get('account', '{}');
          if (!account.token) return catch_error ? reject(Lang.translate('account_login_failed')) : resolve();

          _this.network.silent(_this.api_url + method, resolve, catch_error ? reject : resolve, false, {
            headers: {
              token: account.token,
              profile: account.profile.id
            }
          });
        });
      }
    }, {
      key: "time",
      value: function time(call) {
        this.network.silent(this.api_url + 'time', call, function () {
          call({
            time: Date.now()
          });
        });
      }
    }, {
      key: "m3u",
      value: function m3u(url) {
        var _this2 = this;

        return new Promise(function (resolve, reject) {
          var account = Lampa.Storage.get('account', '{}');
          if (!account.token) return reject(Lampa.Lang.translate('account_login_failed'));

          _this2.network.timeout(20000);

          _this2.network["native"](url, function (str) {
            try {
              var file = new File([str], "playlist.m3u", {
                type: "text/plain"
              });
              var formData = new FormData($('<form></form>')[0]);
              formData.append("file", file, "playlist.m3u");
              $.ajax({
                url: _this2.api_url + 'lampa',
                type: 'POST',
                data: formData,
                async: true,
                cache: false,
                contentType: false,
                timeout: 20000,
                enctype: 'multipart/form-data',
                processData: false,
                headers: {
                  token: account.token,
                  profile
