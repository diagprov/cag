# Crypto Audit Guidelines

I've been doing security audits for quite a few years, both independently and with Kudelski Security, reviewing implementations of cryptography and other security components for a variety of platforms, from smart card applications and dedicated silicon to web and mobile applications, from standards algorithms and elliptic curve arithmetic to consensus protocols and zero-knowledge proofs.
Having worked with many different customers, written many reports, and seen reports from other auditors, I've learnt about things that work and things that don't and wish I had learnt some of these earlier. I also wish all auditors abided to some quality standard and that customers were better informed of what they can expect and demand from auditors.
The following guidelines are thus an attempt to help auditors do a better job and customers get better value for their money, based on my experience (the usual YMMV disclaimer applies).
Lot of things could be said and lot of stories could be told and perhaps at some point I'll write a longer piece, but here I've limited myself to what I think are the most important points.

If you're also doing security audits and would like to comment on these guidelines, please use GitHub Issues.
If you'd like to contribute an entry, please feel free to file a PR.

## For auditors

* **Don't inflate severities**: Sometimes auditors rate an issue high-severity not because its severity is demonstrably high (that is, exploitable), but because it "looks bad" (for example, using MD5), or because they speculate it would be exploitable under a coincidence of events as likely as correctly guessing the private key's value. But the customer does not care about your personal feelings about the bug, they just need meaningful risk rating in order to prioritize fixes and (when applicable) communicate with their users. Never shy away from rating a bug hi-sev if it's actually hi-sev (likely exploitable with baaad consequences, as per the threat model defined), but clearly articulate the exploitation scenario and the business impact (data loss, DoS, etc.).

* **Find solutions, not only problems**: Every issue identified should come with mitigation recommendations. A tautological "fix it" comment is not a valid mitigation recommendation. Instead, be specific and if possible offer a patch. As auditors, it's not always easy to figure out what's the best mitigation, especially to design errors, so don't hesitate to write down several mitigation strategies and discuss them with your customer.

* **Scope flexibly**: Estimating the person-day budget for a "good enough" work is nigh impossible. People use various heuristics, such as N lines of code covered per hours, or N times the time it took to write the code, but such quantitative estimates often end up being of little value compared to qualitative aspects including design complexity, code clarity, language used, auditors' familiarity with the system, and so on. What I found to work well is to provide a range to the customer with a conservative cap (in order to avoid going over budget), but that's not always possible.

* **Do a day if you charge a day**: This should be obvious but it's not always the rule, especially in big companies and consulting firms. A day of work is generally understood as the equivalent of 8 hours of work, so charge the day rate for every 8-hour window of work, not for every day where an employee assigned to the project showed up to the office and worked a couple hours on it.

* **Log your work**: For every hour or block of 2-4 hours of work, keep track of what you've been doing, which files/functions/mechanisms you've analyzed, keep note of your thoughts or failed attack attempts, and share this journal with your team. The customer may demand you to justify how you've spend the time charged, and you should be able to justify it. 

* **4 eyes is better than 2**: It's sometimes natural to distribute the work among team members by splitting the code auditing tasks into different components of the code base (packages, subcrates, etc.), but the problem with this approach is that only up to one person looks at a given line of code, and that nobody gets a full understanding of the interaction between the components. What I found to work well is to assign two persons to a same component, and that everyone gets at least a basic understanding of all the components being reviewed and how they work together. Being two persons instead of just one also leads to discussions that help identify bugs and false positives. It also makes the work feel less boring.

* **Say what you do**: You'll often have a Slack or other group chat established with the customer (if not, try to create one). It's usually good to agree, while preparing the statement of work or during the kick-off meeting, on how this communication channel will be used. I recommend that auditors regularly share the information of what part of the code they're working on, what they find unclear or awkward, and report issues as soon as they find them. This helps catching false positives early and, on the developers' end, planning mitigation.

* **Better a more verbose report**: A verbose report is better than a too laconic one. As a security auditor familiar with attack and exploitation techniques, it's often tempting to skip the details and expect the reader to fill gaps in bugs' description and mitigation recommendations. But the risk is that readers misunderstand the actual issue and fail to address it correctly. Writing down details also helps you spot potential errors or misunderstandings in your analysis.

* **Describe what you haven't found**: A report void of security issues can feel awkward for both the auditor and the customer, the latter worried that the auditor may not have done the job they were paid to. To alleviate such concerns, whether your report includes zero or 100 findings, list the kind of bugs you've been looking for, describe any tools you've used and their configuration, enumerate the properties you've verified (for example, elliptic curve point validation, nonce uniqueness, zero-knowledge property, and so on).

* **Never take things for granted**: When reviewing an algorithm or protocol implementation, always understand what it does and creatively think about what could go wrong; even if the scheme implemented is provably secure, even if it has comprehensive unit tests and full coverage, even if it's formally verified, and even if it's written in Rust. Oftentimes security issues can come from quirks of the language, bad error handling mechanism, unexpected behavior of callers or callees, inacurrate threat model, or other unorthodox causes.

## For customers

* **Agree in advance on the report content**: Don't pay for 3 days of work that were only about (re)writing your specs.

* **Identify what you need and communicate it**, which ; code security, matching with specs, specs correctness. etc.

* **Don't hesitate to challenge the auditors**: Can't be expected to have as deep an understanding of your code base as your developers who've been working on it for the last six months

* **Careful with upsell work packages**: Some companies will try to upsell things such as "threat modeling", "security hardening", "performance optimization", or other more or less relevant subprojects that will translate in increased consulting fees. These can bring great value to the customer if done right and and if the content and goal of the work is clearly understood by both parties beforehand. But it can also turn out to be a scam when pitched by the consulting firm's sales person and signed off by a middle manager of the customer and when no engineer is involved. Such a situation ultimately hurts both sides.