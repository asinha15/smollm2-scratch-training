# SmolLM2-135M Training (ERA HW13)

## Overview
Train the [SmolLM2-135M](https://huggingface.co/HuggingFaceTB/SmolLM2-135M) language model **from scratch** on the provided `input.txt` corpus using Google Colab. The workflow is captured in `smollm2_colab.ipynb` and mirrors the official configuration from the SmolLM repository while adapting it to Colab constraints.

## Contents
- `smollm2_colab.ipynb` – Colab-ready notebook that drives the entire pretraining run.
- `input.txt` – Training corpus (upload this when prompted).
- `stage-1.log`, `stage-2.log` – Example training logs (optional).

## Prerequisites
1. Google account with Colab access (GPU runtime recommended; A100/L4 preferred, T4 works).
2. `input.txt` dataset available locally for upload.
3. Optional: Google Drive for storing checkpoints.

## Running the Notebook
1. Open `smollm2_colab.ipynb` in Google Colab.
2. Switch the runtime to GPU (`Runtime → Change runtime type → GPU`).
3. Execute cells top-to-bottom:
   - Dependency installation pinning NumPy 1.26.4 to avoid binary incompatibilities.
   - Automatic download of `config_smollm2_135M.yaml`.
   - Upload `input.txt` when prompted (uses `files.upload()`).
   - Tokenization into 2,048-token blocks with a dedicated pad token.
4. Launch Stage 1 training (5,000 steps). A sample generation callback prints outputs every 500 steps.
5. Run the “Sync trainer state” cell to copy `trainer_state.json`, optimizer, and scheduler artifacts into `smollm2_135m_runs/stage1_5000_steps/checkpoint-final`.
6. Launch Stage 2 to resume training for 50 additional steps (total 5,050). The notebook automatically reloads weights and optimizer state.

## Outputs
- Checkpoints under `smollm2_135m_runs/`:
  - `stage1_5000_steps/checkpoint-final/` – Model weights after Stage 1 plus trainer state for resumption.
  - `stage2_5050_steps/checkpoint-final/` – Model weights after Stage 2.
- TensorBoard logs located inside each stage directory (`logs/`).

## Continuing Training
- To extend beyond 5,050 steps, adjust `max_steps`, `save_steps`, and resume from the latest `checkpoint-final` directory using the same pattern as Stage 2.
- The notebook detects whether bfloat16 is available; otherwise it defaults to float32 to avoid GradScaler issues on older GPUs.

## Troubleshooting
- **NumPy binary incompatibility:** Restart runtime and rerun the install cell to pin NumPy 1.26.4 before importing `datasets`.
- **FlashAttention missing:** The notebook falls back to standard attention; optionally install `flash-attn` (`pip install flash-attn --no-build-isolation`) before loading the model.
- **Missing `trainer_state.json`:** Always run the sync cell after Stage 1 so Stage 2 can resume optimizers and scheduler state.

## References
- SmolLM configuration: https://raw.githubusercontent.com/huggingface/smollm/main/text/pretraining/smollm2/config_smollm2_135M.yaml

## 5000 steps log
Step	Training Loss	Validation Loss
500	0.221300	7.697577
1000	0.188600	9.043961
1500	0.151400	10.823417
2000	0.127600	12.390569
2500	0.049900	13.886422
3000	0.022100	15.212621
3500	0.010500	15.839497
4000	0.003300	16.389404
4500	0.000700	16.644392

===== Sample @ step 500 =====
First Citizen:
Before we proceed any further, hear me speak.

All:
Speak, speak.

First Citizen:
You are all resolved rather to die than to famish?

All:
Resolved. resolved.

First Citizen:
First, you know Caius Marcius is chief enemy to the people.

All:
We know't, we know't.

First Citizen:
Let us kill him, and we'll have corn at our own price.
Is't a verdict?

All:
No more talking on't; let it :Which;SecondIS?M die, sir my noble- strong if, better, with pardon, so at, to the capWhat,I'll stainsGARET:Or;My power lands,Marrable death, that, who, do that, and your of darkly, to fly? speak's vI think.CORIUS:But,'s friends like made of at him bro par,':CL:How! there, and most lord;Myself.MERC.KING EDWARD:ay, cherish follow.In thy young'd, my motion might, that thou say, your shalt is from him.Which look man, that a hie,, that note's flesh, ass should's rites.So of yourAnd thou speak thy great my words shallare buried'd when before of the consul to her repent, why of,We are upSo so,Where's better, you'll see thy blood, you'll leave
==============================


===== Sample @ step 1000 =====
First Citizen:
Before we proceed any further, hear me speak.

All:
Speak, speak.

First Citizen:
You are all resolved rather to die than to famish?

All:
Resolved. resolved.

First Citizen:
First, you know Caius Marcius is chief enemy to the people.

All:
We know't, we know't.

First Citizen:
Let us kill him, and we'll have corn at our own price.
Is't a verdict?

All:
No more talking on't; let it .PET Henry is, ineth; good time through.A faithful would have.To become does.QUE greaterThiedONTNot upon,My Lord; words.S upon, be the woman.Nurse:G have that thy consent.At that our, by heaven have, now RichardOLYC, content shun me soANT,By my art life notWhat. Rateth; usat; and such my us your far, but not we have twenty more again,Ayself me o' the purpose thus?The trib thing is my us know him,When will give him?Wh will took to be pardon me:LUC in cannot, give him will meet the faults.From me forAlasorrow.Give me to me for a happy; and cheek, boy.RICHARDure:Very well both of all better far.Vks of all,And this is it no opposite, and them, hear's untGLOUCio,And acquainted
==============================


===== Sample @ step 1500 =====
First Citizen:
Before we proceed any further, hear me speak.

All:
Speak, speak.

First Citizen:
You are all resolved rather to die than to famish?

All:
Resolved. resolved.

First Citizen:
First, you know Caius Marcius is chief enemy to the people.

All:
We know't, we know't.

First Citizen:
Let us kill him, and we'll have corn at our own price.
Is't a verdict?

All:
No more talking on't; let it M charge by the must good HENRY VILADYHERMlF attend him,, come' have;I throw forward again and with stay at your lord and my his that comfort isAnd say, I will yield the s fit so cheer of theBecause.N beat the brid here did.Nurse:Th her; if rack'd your may part.And the duty his great painsDUKE VINCENTIO:Have lo, and your honourTis them,PAULINAHe to France.I turn, I say live to askWhichhow no;Now, was said,F choke, defend of your kinsly; if that of youUp,And all, and you nor no more fortune andIn le ofInto himself, if cheer,Is wooed to our peril and throne newWhere! that bride,Greatnow shall whatO: and I have been,F sound lord, say you thing from graceAnd the house, and
==============================


===== Sample @ step 2000 =====
First Citizen:
Before we proceed any further, hear me speak.

All:
Speak, speak.

First Citizen:
You are all resolved rather to die than to famish?

All:
Resolved. resolved.

First Citizen:
First, you know Caius Marcius is chief enemy to the people.

All:
We know't, we know't.

First Citizen:
Let us kill him, and we'll have corn at our own price.
Is't a verdict?

All:
No more talking on't; let it Pl eff. Giveest me say, he will thou you.The time the gates, Lord, IWhere;, hands you. Give me but I danced you asumbling my horseest me.is you shallre me as me the seaest me.How I have, on if thou must--God strange's name broken again of gau down the realm I thou,ly ofkn home.I go, my lord, and must have ourSt ourself tellAs it, my very Thou me's loose.and shall I mistrust. Their's blood was on within'd;Asaf to enter himself before O simple forbidart to great mustO,est me for thyier.QUEEN ELIZABETH:Thus, since with blood honestest me to as thou must say; O heavens's, to nothing glass, lie; when grief he  sorrows nameKING, taught me! SirOf me her native treason in love.sh both wilt'st thouThey are sure
==============================


===== Sample @ step 2500 =====
First Citizen:
Before we proceed any further, hear me speak.

All:
Speak, speak.

First Citizen:
You are all resolved rather to die than to famish?

All:
Resolved. resolved.

First Citizen:
First, you know Caius Marcius is chief enemy to the people.

All:
We know't, we know't.

First Citizen:
Let us kill him, and we'll have corn at our own price.
Is't a verdict?

All:
No more talking on't; let it  Be deal stock;en flint is:Here in the must of packry,G of or, Queen you huntedUMN Musician:Did keep,-- sleep I must: good wish my husband: what are ear,atures knows.Of set tongue sleep jealous.He set in the en of it: expect sweet or care of in the brotherANIO:G blood that readiness more veil a How dumbeness: of inant wall. Is my gracious self, sir, that not.I will be withinSt.To set knows is what the bab conquer holds bro prince and bed self,--I find, I will not.But or not be pictures to such will expectHere:He such the nuain he, seven HENRY VI:And summon him will.High knowsst the infant shalt,:Who I will not with me. Even thou soilost thou speakNot with dis numb,L falsehood in the happy blessed plotio'st it in owl, that set will
==============================


===== Sample @ step 3000 =====
First Citizen:
Before we proceed any further, hear me speak.

All:
Speak, speak.

First Citizen:
You are all resolved rather to die than to famish?

All:
Resolved. resolved.

First Citizen:
First, you know Caius Marcius is chief enemy to the people.

All:
We know't, we know't.

First Citizen:
Let us kill him, and we'll have corn at our own price.
Is't a verdict?

All:
No more talking on't; let it  business make on favour st hearing grave advise, might hathous order now be v.Go silence, good all plant the town against that ever in it was DidBy sweet why, to FranceC Take my your flesh: for; have with death should, will heaven! giver'd toThe age- heard,Your be her my this time's husband.GLOUCESTER:I am to our comfort that I did,JULI grant.Go still a sweeter yet oftwith it!'If, boys Berkeley, gentlemen there?Well, had; I may be,Nurse:He request, and am; and am unto you a thornKINGBENVOLIO:And make set it a wrongs andial shame, and she lockant'd to prison. LEWouch in this yet graciousUMNIA:So the sea, and lock up,? lim three daughters!do with Edward raised outward much.Welcome, it mightush, sweet son Petruchio,
==============================


===== Sample @ step 3500 =====
First Citizen:
Before we proceed any further, hear me speak.

All:
Speak, speak.

First Citizen:
You are all resolved rather to die than to famish?

All:
Resolved. resolved.

First Citizen:
First, you know Caius Marcius is chief enemy to the people.

All:
We know't, we know't.

First Citizen:
Let us kill him, and we'll have corn at our own price.
Is't a verdict?

All:
No more talking on't; let it  dancehyont.NORis, to change you for allen and supply contradiction descent!But, Lancaster was I sw in, have b now possess, have yetthood to bI the restions are mostROMEO: common cheeksouncingVery sessions to me?Why slew most,That will bether, my sweet, in place, dress, worthy husband.Akindurl'd Master with his rev, my lord, wilt calmdone by up at wall am.I am carake to the happy brain silence:With bag Verona; have the people.Let you was of ' a pieceANIO:H MasterORTHUMNIA:Did only Vol! O something, it be grief we old not people wasTo see beseech you.Y are to't now in aitors unknown safe, sound clouds Aufidius, sound with't very well a poorst thus I have in spite of a sweet sweet sweet face something,Why all banish claim,
==============================

## 50 additional steps log
Step	Training Loss	Validation Loss
***** train metrics *****
  epoch                    =   388.2963
  total_flos               = 49102244GF
  train_loss               =        0.0
  train_runtime            = 0:00:53.19
  train_samples_per_second =     759.45
  train_steps_per_second   =     94.931
 [12/12 00:00]
***** eval metrics *****
  epoch                   =   388.2963
  eval_loss               =    16.6908
  eval_runtime            = 0:00:00.49
  eval_samples_per_second =     24.232
  eval_steps_per_second   =     24.232
Stage 2 completed. Final checkpoint stored under: smollm2_135m_runs/stage2_5050_steps
