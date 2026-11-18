# nezuko-baileys

فورك مستقل باسم `nezuko-baileys` مبني على `@rexxhayanasi/elaina-baileys@1.4.7`، مع إبقاء طبقة VoIP المدمجة وتوفير مسار ESM ثابت:

```js
import makeWASocket, { useMultiFileAuthState } from 'nezuko-baileys';
import { makeVoipClient } from 'nezuko-baileys/voip';
```

## تثبيت محلي من GitHub

```bash
npm install github:mzml-gg/nezuko-baileys
```

## مثال مكالمة صوتية

```js
import makeWASocket, { useMultiFileAuthState } from 'nezuko-baileys';
import { makeVoipClient } from 'nezuko-baileys/voip';

const { state, saveCreds } = await useMultiFileAuthState('./session');
const sock = makeWASocket({ auth: state, printQRInTerminal: true });
sock.ev.on('creds.update', saveCreds);

sock.ev.on('connection.update', async ({ connection }) => {
  if (connection !== 'open') return;

  const voip = await makeVoipClient(sock, { debug: false });
  const call = await voip.call('201000000000', {
    audioSource: './audio.mp3',
    durationMs: 120000
  });

  call.on('connected', () => console.log('call connected'));
  call.on('ended', (reason) => console.log('call ended:', reason));
});
```

## مثال فيديو أو playlist

```js
const voip = await makeVoipClient(sock);
await voip.call('201000000000', {
  video: true,
  videoSource: './poster.jpg',
  playlist: ['./audio.mp3'],
  durationMs: 120000
});
```

## ما تم دمجه

الحزمة مبنية على Elaina لأنها تحتوي بالفعل على محرك VoIP كامل، وطبقة WASM، وsignaling، وaudio/video feeders. تم إضافة مسار `nezuko-baileys/voip` كواجهة مستقرة ومتوافقة مع ESM.

حزمتا Elaina وHirooSy تحتويان على محركات VoIP مستقلة ومختلفة داخلياً. تحميل المحركين معاً في نفس socket يسبب تعارض handlers وsignaling، لذلك لا يتم تشغيل محركين متنافسين. تم حفظ بيانات المصدر والتراخيص في `LICENSE.md` و`NOTICE.md`، وتم تسجيل المصدرين في `package.json`.

## الاختبار

```bash
npm install
npm test
npm pack --dry-run
```

يحتاج تشغيل الصوت والفيديو إلى الموارد الموجودة داخل الحزمة، وقد تحتاج بعض حالات الوسائط إلى `ffmpeg`/`ffprobe` حسب نوع الملف.
