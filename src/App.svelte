<script>
  let showModal = false;

  // نموذج البيانات مع مدة السداد مضافة
  let form = { name: '', email: '', amount: '', duration: '', salaryDate: '' };

  let result = '';
  let errors = {};

  const interest = 0.05; // فائدة بسيطة 5%

  function validate() {
    const errs = {};
    if (!form.name.trim()) errs.name = 'الاسم مطلوب';
    if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email)) errs.email = 'إيميل غير صالح';
    if (!form.amount || Number(form.amount) <= 0) errs.amount = 'أدخلي مبلغًا صالحًا';
    if (!form.duration || Number(form.duration) < 1) errs.duration = 'مدة السداد مطلوبة';
    if (!form.salaryDate) errs.salaryDate = 'تاريخ الراتب مطلوب';
    return errs;
  }

  function submit() {
    errors = validate();

    if (Object.keys(errors).length === 0) {
      const principal = Number(form.amount);
      const months = Number(form.duration);
      const total = principal + (principal * interest);
      const monthly = total / months;

      result =
        `تم تأكيد الطلب.\n` +
        `سيتم استقطاع ${monthly.toFixed(2)}$ شهريًا لمدة ${months} شهرًا.\n` +
        `المبلغ الكلي مع الفائدة: ${total.toFixed(2)}$.\n` +
        `أول استقطاع سيكون بتاريخ ${form.salaryDate}.`;

      // بعد 3 ثوانٍ تظهر الرسالة الإضافية
      setTimeout(() => {
        result += `\nستصلك رسالة بوقت استلامك للسلفة قريبًا، شكرًا لك.`;
      }, 3000);
    } else {
      result = 'يرجى تصحيح الأخطاء أعلاه قبل تأكيد الطلب.';
    }
  }

  function resetAndOpen() {
    form = { name: '', email: '', amount: '', duration: '', salaryDate: '' };
    errors = {};
    result = '';
    showModal = true;
  }
</script>

<style>
  :root {
    --bg: #F6F1E7;          /* بيج فاتح */
    --card: #FFFFFF;        /* أبيض للكروت */
    --text: #2D2A26;        /* نص غامق هادئ */
    --muted: #6D6761;       /* نص ثانوي */
    --border: #E7E1D6;      /* حدود ناعمة */
    --brown: #7B4B2A;       /* جوزي */
    --brown-hover: #6A3F24; /* جوزي أغمق */
    --success-bg: #F3F8F1;  /* نجاح ناعم */
    --success-border: #CDE4C4;
  }

  html, body {
    margin: 0;
    padding: 0;
    background: var(--bg);
    color: var(--text);
    font-family: 'Segoe UI', Tahoma, Arial;
  }

  /* إطار موبايل */
  .phone {
    width: 390px;
    max-width: 100%;
    margin: 24px auto;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 24px;
    box-shadow: 0 8px 24px rgba(0,0,0,0.08);
    overflow: hidden;
  }

  /* شريط علوي */
  .topbar {
    background: var(--card);
    border-bottom: 1px solid var(--border);
    padding: 14px 16px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .brand {
    font-weight: 700;
    font-size: 1rem;
    letter-spacing: 0.2px;
  }
  .hint {
    color: var(--muted);
    font-size: 0.85rem;
  }

  .content { padding: 16px; display: grid; gap: 12px; }

  .cta {
    padding: 12px 16px;
    border-radius: 14px;
    border: 1px solid var(--border);
    background: var(--bg);
    color: var(--text);
  }

  .btn {
    display: inline-block;
    width: 100%;
    padding: 12px 16px;
    border-radius: 12px;
    border: 1px solid var(--brown);
    background: var(--brown);
    color: #fff;
    font-weight: 600;
    cursor: pointer;
    transition: background 0.15s ease;
  }
  .btn:hover { background: var(--brown-hover); }

  /* مودال */
  .backdrop {
    position: fixed; inset: 0; background: rgba(0,0,0,0.35);
    display: grid; place-items: center; z-index: 50;
  }
  .modal {
    width: 100%; max-width: 380px;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 20px;
    overflow: hidden;
  }
  .modal-header, .modal-footer {
    padding: 14px 16px;
    border-bottom: 1px solid var(--border);
  }
  .modal-footer {
    border-top: 1px solid var(--border);
    border-bottom: none;
    display: flex;
    gap: 8px;
    justify-content: space-between;
  }
  .modal-body { padding: 14px 16px; display: grid; gap: 10px; }

  .label { font-weight: 600; font-size: 0.95rem; }
  .input {
    width: 100%;
    padding: 10px 12px;
    border: 1px solid var(--border);
    border-radius: 12px;
    background: #fff;
  }
  .error-text { color: #C0392B; font-size: 0.85rem; }

  .success {
    background: var(--success-bg);
    border: 1px solid var(--success-border);
    color: #245C2E;
    padding: 10px 12px;
    border-radius: 12px;
    font-size: 0.95rem;
    white-space: pre-line; /* لعرض الأسطر المتعددة في الرسالة */
  }

  .btn-secondary {
    background: #fff;
    color: var(--brown);
    border: 1px solid var(--border);
  }
  .btn-secondary:hover {
    background: #f9f6f1;
  }
</style>

<div class="phone" dir="rtl">
  <div class="topbar">
    <div class="brand">سلفة شخصية</div>
    <div class="hint">أهلاً بك</div>
  </div>

  <div class="content">
    <div class="cta">
      اطلبي السلفة وسيتم الاستقطاع تلقائيًا عند تاريخ الراتب الذي تحدّدينه.
    </div>
    <button class="btn" on:click={resetAndOpen}>اطلب سلفة الآن</button>
  </div>
</div>

{#if showModal}
  <div class="backdrop" on:click={() => showModal = false}>
    <div class="modal" on:click|stopPropagation>
      <div class="modal-header">
        <strong>طلب سلفة</strong>
        <div class="hint">أدخلي بياناتك لحساب الاستقطاع</div>
      </div>

      <div class="modal-body">
        <div>
          <div class="label">مدة السداد (بالأشهر)</div>
          <input class="input" type="number" min="1" max="36" placeholder="مثال: 12" bind:value={form.duration} />
          {#if errors.duration}<div class="error-text">{errors.duration}</div>{/if}
        </div>

        <div>
          <div class="label">الاسم الكامل</div>
          <input class="input" placeholder="اكتبي اسمك" bind:value={form.name} />
          {#if errors.name}<div class="error-text">{errors.name}</div>{/if}
        </div>

        <div>
          <div class="label">الإيميل</div>
          <input class="input" type="email" placeholder="example@email.com" bind:value={form.email} />
          {#if errors.email}<div class="error-text">{errors.email}</div>{/if}
        </div>

        <div>
          <div class="label">مبلغ السلفة (بالدولار)</div>
          <input class="input" type="number" min="1" placeholder="مثال: 3000" bind:value={form.amount} />
          {#if errors.amount}<div class="error-text">{errors.amount}</div>{/if}
        </div>

        <div>
          <div class="label">تاريخ الراتب</div>
          <input class="input" type="date" bind:value={form.salaryDate} />
          {#if errors.salaryDate}<div class="error-text">{errors.salaryDate}</div>{/if}
        </div>

        {#if result}
          <div class="success">{result}</div>
        {/if}
      </div>

      <div class="modal-footer">
        <button class="btn-secondary btn" on:click={() => showModal = false}>إغلاق</button>
        <button class="btn" on:click={submit}>تأكيد الطلب</button>
      </div>
    </div>
  </div>
{/if}