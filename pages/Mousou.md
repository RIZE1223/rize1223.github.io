&nbsp;
<img src="../images/mou.png">







# 概要

## コンセプト
カードを出してユニットを出し自分の陣地を守り敵の陣地を攻撃する。

## 制作意図
We,areTECH.C(卒業、進級制作展)2022出展用ゲームのチーム制作作品

## プラットフォーム
PC

## 使用言語
C#
Unity 2020.2.5f1

# 担当箇所

```cs
public class SoundManager : MonoBehaviour
{
    [SerializeField] AudioSource bgmAudioSource;
    [SerializeField] AudioSource seAudioSource;

    [SerializeField] List<BGMSoundData> bgmSoundDatas;
    [SerializeField] List<SESoundData> seSoundDatas;

    public float masterVolume = 1;
    public float bgmMasterVolume = 1;
    public float seMasterVolume = 1;

    public static SoundManager Instance { get; private set; }

    private void Awake()
    {
        if (Instance == null)
        {
            Instance = this;
            DontDestroyOnLoad(gameObject);
        }
        else
        {
            Destroy(gameObject);
        }
    }

    public void PlayBGM(BGMSoundData.BGM bgm)
    {
        BGMSoundData data = bgmSoundDatas.Find(data => data.bgm == bgm);
        bgmAudioSource.clip = data.audioClip;
        bgmAudioSource.volume = data.volume * bgmMasterVolume * masterVolume;
        bgmAudioSource.Play();
    }


    public void PlaySE(SESoundData.SE se)
    {
        SESoundData data = seSoundDatas.Find(data => data.se == se);
        seAudioSource.volume = data.volume * seMasterVolume * masterVolume;
        seAudioSource.PlayOneShot(data.audioClip);
    }

}

[System.Serializable]
public class BGMSoundData
{
    public enum BGM
    {
        Title,
        Battle, // これがラベルになる
    }

    public BGM bgm;
    public AudioClip audioClip;
    [Range(0, 1)]
    public float volume = 1;
}

[System.Serializable]
public class SESoundData
{
    public enum SE
    {
        Attack,
        打ち上げ花火,
        カードをめくる,
        財布をジャラツ, // これがラベルになる
    }

    public SE se;
    public AudioClip audioClip;
    [Range(0, 1)]
    public float volume = 1;
}
```
ゲームのBGMとボタンを押した時のSEなどを管理する処理を作りました。