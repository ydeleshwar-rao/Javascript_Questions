// UnitAndTypeConversions.jsx
// Real-world conversions commonly used in React projects

import React from "react";

// =============================
// STRING -> NUMBER CONVERSIONS
// =============================

// "1280px" -> 1280
export const pxToNumber = (value) => parseInt(value, 10);

// "1.5rem" -> 1.5
export const remToNumber = (value) => parseFloat(value);

// "75%" -> 75
export const percentToNumber = (value) => parseFloat(value);

// "00123" -> 123
export const stringToInt = (value) => parseInt(value, 10);

// "12.45" -> 12.45
export const stringToFloat = (value) => Number(value);

// =============================
// NUMBER -> STRING WITH UNITS
// =============================

// 1280 -> "1280px"
export const numberToPx = (value) => `${value}px`;

// 1.5 -> "1.5rem"
export const numberToRem = (value) => `${value}rem`;

// 75 -> "75%"
export const numberToPercent = (value) => `${value}%`;

// =============================
// BOOLEAN CONVERSIONS (FOR FORMS)
// =============================

// "true" -> true
export const stringToBoolean = (value) => value === "true";

// true -> "true"
export const booleanToString = (value) => String(value);

// =============================
// DATE CONVERSIONS
// =============================

// "2026-01-08" -> Date object
export const stringToDate = (value) => new Date(value);

// Date -> "2026-01-08"
export const dateToString = (date) => date.toISOString().split("T")[0];

// =============================
// ARRAY / OBJECT CONVERSIONS
// =============================

// "1,2,3" -> [1,2,3]
export const csvToNumberArray = (value) =>
  value.split(",").map((item) => Number(item));

// [1,2,3] -> "1,2,3"
export const arrayToCsv = (arr) => arr.join(",");

// =============================
// JSON CONVERSIONS (API DATA)
// =============================

// "{\"name\":\"John\"}" -> object
export const jsonToObject = (value) => JSON.parse(value);

// object -> "{...}"
export const objectToJson = (value) => JSON.stringify(value);

// =============================
// LOCAL STORAGE CONVERSIONS
// =============================

export const saveToLocalStorage = (key, value) => {
  localStorage.setItem(key, JSON.stringify(value));
};

export const getFromLocalStorage = (key) => {
  const value = localStorage.getItem(key);
  return value ? JSON.parse(value) : null;
};

// =============================
// DEMO COMPONENT
// =============================

export default function ConversionDemo() {
  const breakpoint = "1280px";
  const bpNumber = pxToNumber(breakpoint);

  return (
    <div style={{ padding: 20 }}>
      <h2>Real Project Conversions Demo</h2>
      <p>String breakpoint: {breakpoint}</p>
      <p>Converted number: {bpNumber}</p>
      <p>Back to px: {numberToPx(bpNumber)}</p>
    </div>
  );
}
