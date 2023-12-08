# Alexey Shemiakin
## Junior frontend developer
![Alexey Shemiakin](https://avatars.githubusercontent.com/u/141009527?v=4)
### Contacts
* Phone number: +66 63 506 7320
* Email: Enotinof@yandex.ru
* Telegram: @enotinof
* [Facebook](https://www.facebook.com/profile.php?id=100006669213329)
### About me
I'm thirty eight years old. I have my own small business that needs improvement through web technologies. I would like to learn by creating my own web application for my company and perhaps become profocient in full-stack development.
### Basic skills
* HTML
* CSS
* JavaScript
* TypeScript
* Git
* React
### Code exemple
Here is one example of my React code using the component "InputSymbol" with TypeScript:
```
import {FC, PropsWithChildren, CSSProperties, ChangeEvent, useState, useMemo} from 'react';
import styles from './styles.module.css';
import InputSymbolProps from './inputSymbolProps';
import useValidation from '../custom-hooks/useValidation/useValidation';
import useResizeListener from '../../custom-hooks/useResizeListener/useResizeListener';
import handleInputFocus from '../../custom-hooks/handleInputFocus/handleInputFocus';

  const InputSymbol: FC<PropsWithChildren<InputSymbolProps>> = function({ id, type, header, headerPosition, placeholder, width, footer, headerWidth, inputWidth, footerWidth, binding,
  value, onChange, handleBlur, validBorder, validError }) {

  const {errorT, errorB} = useValidation(binding, value, validError);

  const [headPosition, setHeadPosition] = useState<"up" | "ahead">(headerPosition);

  const inputChange = (event: ChangeEvent<HTMLInputElement>) => {
    onChange(event.target.value);
  }

  const footerWidthNumber = footerWidth ? parseInt(footerWidth) : 0;

  const classMyLabelDown = `${styles.myLabelColumn} ${headPosition === 'ahead' && errorT && !validBorder ? `${styles.myLabelRow}` : `${styles.LabelNone}`}`;
  const classInput = `${styles.input} ${headPosition === 'up' ? ` ${styles.marginInput}` : ''} ${!footer || !footerWidthNumber ? `${styles.withoutFooter}` :
      ''} ${validBorder && errorB ? `${styles.inputError}` : ''}`;
  const classErrorUp = `${styles.MyBinding} ${headPosition === 'ahead' || !errorT || validBorder ? `${styles.ErrorRow} ${styles.erNone}` : `${styles.Error}`}`;
  const classErrorDown = `${styles.MyBinding} ${errorT ? `${styles.ErrorRow}` : `${styles.Error} ${styles.erNone}`}`;

  type MemoResult = [CSSProperties, CSSProperties, CSSProperties, CSSProperties, string, string, string];

  const calculateMemo = useMemo<MemoResult>(() => {

    const headerWidthNumber = headerWidth ? parseInt(headerWidth) : 0;
    const inputWidthNumber = inputWidth ? parseInt(inputWidth) : 0;
    const sizeSum = (headerWidthNumber + inputWidthNumber + footerWidthNumber) == 100 ? true : false;
    const styleMyMainDiv: CSSProperties = width ? { width: `${width}px` } : { width: 'calc(100% - 10px)' };
    const styleHeader: CSSProperties = headPosition === 'ahead' ? sizeSum ? { width: `${headerWidth}%` } : { width: '40%' } : { width: '100%' };
    const styleInput: CSSProperties = headPosition === 'ahead' ? sizeSum ? { width: `${inputWidth}%` } : { width: '50%' } : { width: '100%' };
    const styleFooter: CSSProperties = headPosition === 'ahead' ? sizeSum ? { width: `${footerWidth}%` } : { width: '10%' } : { width: '100%' };

    const classMyLabelUp = `${styles.myLabelColumn} ${headPosition === 'ahead' ? `${styles.myLabelRow}` : ''}`;
    const bindingClass = `${styles.MyBinding} ${binding ? `` : `${styles.Hidden}`}`;
    const classFooter = !footer || !footerWidthNumber ? `${styles.MyFooterNone}` : ``;

    return [styleMyMainDiv, styleHeader, styleInput, styleFooter, classMyLabelUp, bindingClass, classFooter];

  }, [headerPosition, width, footer, headerWidth, inputWidth, footerWidth, binding, validBorder]);

  const [styleMyMainDiv, styleHeader, styleInput, styleFooter, classMyLabelUp, bindingClass, classFooter] = calculateMemo;

  const windowWidthAdaptation = () => setHeadPosition(window.innerWidth <= 380 ? 'up' : headerPosition);

  useResizeListener(windowWidthAdaptation);

  return (
    <div className={styles.myDive} style={styleMyMainDiv}>
      <label className={classMyLabelUp}>
        <div style={styleHeader}>
          {header && header + ':'}
          <span className={bindingClass}> *</span>
        </div>
        <div style={styleInput}>
          <input
            id={id}
            type={type}
            value={value}
            onChange={inputChange}
            onFocus={handleInputFocus}
            placeholder={placeholder}
            className={classInput}
            onBlur={handleBlur}
          />
        </div>
        <div className={classFooter} style={styleFooter}>
          {footer && '   ' + footer}
        </div>
        <div className={classErrorUp}>{errorT}</div>
      </label>

      <label className={classMyLabelDown} htmlFor={id}>
        <div className={styles.toHidden} style={styleHeader}>
        </div>
        <div style={styleInput}>
          <div className={classErrorDown}>{errorT}</div>
        </div>
        <div className={`${classFooter} ${styles.toHidden}`} style={styleFooter}>
        </div>
      </label>
    </div>
  );
};

export default InputSymbol;
```
### Courses
Only self-study
### Languages
* Russian (native)
* English (pre intermediate, according to https://www.efset.org)