---
layout: post
title: React Jest Enzyme Test Cheatsheet
categories: react jest enzyme tdd
permalink: /:title
---

Comparing Jest vs. Enzyme in getting "This is counter app" from an h1 in App.

describe("Counter Testing", () => {
  test("render the title of counter", () => {
    const wrapper = shallow(<App />);
    expect(wrapper.find("h1").text()).toContain("This is counter app.");

    // Old Jest code, less readable
    // const { getByText } = render(<App />);
    // const linkElement = getByText("This is counter app.");
    // expect(linkElement).toBeInTheDocument();
  });
});

Source: [https://www.youtube.com/watch?v=-bmdf1oATQo](https://www.youtube.com/watch?v=-bmdf1oATQo)