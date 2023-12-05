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

/**
 * # Поле для ввода
 * Компонент предназначен для ввода символов разного типа.
 * @param id Уникальный индекс компонента.
 * @param InputSymbolProps Пропсы компонента.
 * @param type Тип данных в поле.
 * @param header Текст наименования поля.
 * @param headerPosition Положение элементов компонента относительно друг друга.
 * @param placeholder Подсказка внутри поля.
 * @param width Общая ширина всего компонента (px).
 * @param headerWidth Ширина наименования поля (%).
 * @param inputWidth Ширина поля для ввода данных (%).
 * @param footerWidth Ширина текста в конце поля (%).
 * @param binding Обязательное поле, если значение свойства равно true.
 * @param value Значение поля.
 * @param onChange Обработчик изменения значения поля.
 * @param handleBlur Функция вызова при потере фокуса.
 * @param validBorder Вместо текста ошибки валидации будет выделено поле для ввода, если значение свойства равно true.
 * @param validError Текст ошибки внешней валидации.
 * @returns JSX элемент.
 */
  const InputSymbol: FC<PropsWithChildren<InputSymbolProps>> = function({ id, type, header, headerPosition, placeholder, width, footer, headerWidth, inputWidth, footerWidth, binding,
  value, onChange, handleBlur, validBorder, validError }) {

  // Применяем кастомный хук useValidation для валидации обязательного поля
  const {errorT, errorB} = useValidation(binding, value, validError);

  // объявляем состояние расположения элементов относительно друг друга
  const [headPosition, setHeadPosition] = useState<"up" | "ahead">(headerPosition);

  // объявляем обработчик отслеживания изменения значения в поле
  const inputChange = (event: ChangeEvent<HTMLInputElement>) => {
    onChange(event.target.value);
  }

  /* Устанавливаем инлайн-стиль для footer, используемый в определении класса classInput, который в свою очередь зависит от значения поля,
  и поэтому не должен быть заключен в useMemo */
  const footerWidthNumber = footerWidth ? parseInt(footerWidth) : 0;

  // устанавливаем классы, которые зависят от значения поля, и поэтому не должны быть заключены в useMemo
  const classMyLabelDown = `${styles.myLabelColumn} ${headPosition === 'ahead' && errorT && !validBorder ? `${styles.myLabelRow}` : `${styles.LabelNone}`}`;
  const classInput = `${styles.input} ${headPosition === 'up' ? ` ${styles.marginInput}` : ''} ${!footer || !footerWidthNumber ? `${styles.withoutFooter}` :
      ''} ${validBorder && errorB ? `${styles.inputError}` : ''}`;
  const classErrorUp = `${styles.MyBinding} ${headPosition === 'ahead' || !errorT || validBorder ? `${styles.ErrorRow} ${styles.erNone}` : `${styles.Error}`}`;
  const classErrorDown = `${styles.MyBinding} ${errorT ? `${styles.ErrorRow}` : `${styles.Error} ${styles.erNone}`}`;

  /* Вычисления классов и других параметров, зависящих от пропсов и необходимых только на момент сборки приложения, заключим в useMemo,
  но сначала объявим тип для возвращаемого массива переменных, чтобы типы всех переменных не объединились: */ 
  type MemoResult = [CSSProperties, CSSProperties, CSSProperties, CSSProperties, string, string, string];

  const calculateMemo = useMemo<MemoResult>(() => {

    // устанавливаем инлайн-стили динамических размеров в зависимости от пропсов
    const headerWidthNumber = headerWidth ? parseInt(headerWidth) : 0;
    const inputWidthNumber = inputWidth ? parseInt(inputWidth) : 0;
    const sizeSum = (headerWidthNumber + inputWidthNumber + footerWidthNumber) == 100 ? true : false;
    const styleMyMainDiv: CSSProperties = width ? { width: `${width}px` } : { width: 'calc(100% - 10px)' };
    const styleHeader: CSSProperties = headPosition === 'ahead' ? sizeSum ? { width: `${headerWidth}%` } : { width: '40%' } : { width: '100%' };
    const styleInput: CSSProperties = headPosition === 'ahead' ? sizeSum ? { width: `${inputWidth}%` } : { width: '50%' } : { width: '100%' };
    const styleFooter: CSSProperties = headPosition === 'ahead' ? sizeSum ? { width: `${footerWidth}%` } : { width: '10%' } : { width: '100%' };

    // устанавливаем классы компоеннтов в зависимости от пропсов
    const classMyLabelUp = `${styles.myLabelColumn} ${headPosition === 'ahead' ? `${styles.myLabelRow}` : ''}`;
    const bindingClass = `${styles.MyBinding} ${binding ? `` : `${styles.Hidden}`}`;
    const classFooter = !footer || !footerWidthNumber ? `${styles.MyFooterNone}` : ``;

    return [styleMyMainDiv, styleHeader, styleInput, styleFooter, classMyLabelUp, bindingClass, classFooter];

  }, [headerPosition, width, footer, headerWidth, inputWidth, footerWidth, binding, validBorder]);

  const [styleMyMainDiv, styleHeader, styleInput, styleFooter, classMyLabelUp, bindingClass, classFooter] = calculateMemo;

  // функция изменения представления компонента в зависимости от ширины экрана пользователя
  const windowWidthAdaptation = () => setHeadPosition(window.innerWidth <= 380 ? 'up' : headerPosition);

  // Применяем кастомный хук useResizeListener для мониторинга изменений размеров окна браузера и передаем функцию windowWidthAdaptation параметром
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