### React-custom-hooks

## useInput

```
import { useState } from "react";

export const useInput = (initialValue) => {
  const [value, setValue] = useState(initialValue);

  const handleOnChange = (event) => {
    setValue(event.target.value);
  };

  return {
    value,
    handleOnChange,
  };
};

```

## useHover

```
import { useEffect, useState } from "react";

export const useHover = (ref) => {
  const [isHovering, setIsHovering] = useState(false);

  const on = () => setIsHovering(true);
  const off = () => setIsHovering(false);

  useEffect(() => {
    if (!ref.current) return;

    const node = ref.current;

    node.addEventListener("mouseenter", on);
    node.addEventListener("mousemove", on);
    node.addEventListener("mouseleave", off);

    return function () {
      node.removeEventListener("mouseenter", on);
      node.removeEventListener("mousemove", on);
      node.removeEventListener("mouseleave", off);
    };
  }, []);

  return isHovering;
};

```

## useScroll

```
import { useEffect, useRef } from "react";

export const useScroll = (parentRef, childRef, callback) => {
  const observer = useRef();

  useEffect(() => {
    const options = {
      root: parentRef.current,
      rootMargin: "0px",
      threshold: 0,
    };
    observer.current = new IntersectionObserver(([target]) => {
      if (target.isIntersecting) {
        callback();
      }
    }, options);

    observer.current.observe(childRef.current);

    return function () {
      observer.current.unobserve(childRef.current);
    };
  }, [callback]);
};

```

## useDebounce

```
import { useCallback, useRef } from "react";

export const useDebounce = (callback, delay) => {
  const timer = useRef();

  const debouncedCallback = useCallback(
    (...args) => {
      if (timer.current) {
        clearTimeout(timer.current);
      }
      timer.current = setTimeout(() => {
        callback(...args);
      }, delay);
    },
    [callback, delay],
  );

  return debouncedCallback;
};

```

## useRequest

```
import { useEffect, useState } from "react";

export const useRequest = (request) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");

  useEffect(() => {
    setLoading(true);
    request()
      .then((response) => setData(response.data))
      .catch((error) => setError(error))
      .finally(() => setLoading(false));
  }, []);

  return [data, loading, error];
};

```