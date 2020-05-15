FROM alpine:3.11.6
# 2020年05月12日 升级为基于3.11.6构建

MAINTAINER 王立鹏 强化alpine <wanglipeng@snsoft.com.cn>
# Build:docker build -t wlp7662973/sn-alpine:3.11.6 ./sn-alpine
# 测试:docker run -it --rm --name test_os wlp7662973/sn-alpine:3.11.6
# push:docker push wlp7662973/sn-alpine:3.11.6

## glibc的下载地址
ENV GLIBC_TGZ_URLS \
# 公司内下载地址,上传开源社区注释掉该地址
#    https://n.esnsoft.cn/alpine-pkg-glibc/glibc-2.31-r0.apk \
# sgerrand下载地址
    https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.31-r0/glibc-2.31-r0.apk

ENV GLIBC_BIN_URLS \
# 公司内下载地址,上传开源社区注释掉该地址
#    https://n.esnsoft.cn/alpine-pkg-glibc/glibc-bin-2.31-r0.apk \
# sgerrand下载地址
    https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.31-r0/glibc-bin-2.31-r0.apk

ENV GLIBC_I18N_URLS \
# 公司内下载地址,上传开源社区注释掉该地址
#    https://n.esnsoft.cn/alpine-pkg-glibc/glibc-i18n-2.31-r0.apk \
# sgerrand下载地址
    https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.31-r0/glibc-i18n-2.31-r0.apk

#安装必要的工具
RUN set -eux; \
    # 设置安装源地址为阿里源
    sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories;\
    # 安装必要的软件包
    apk add --no-cache tzdata curl ca-certificates openssl libuuid bash tar zip unzip bzip2 wget busybox-extras sysstat tcpdump ethtool nload iftop nethogs bmon vnstat bwm-ng darkstat nmap strace; \
	# 设置时区为Shanghai,设置完时区,删除tzdata
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
	&& echo "Asia/Shanghai" > /etc/timezone \
	&& apk del tzdata; \
	# 创建ll命令
	{ \
	    echo 'alias ll="ls -l --color=auto"'; \
	    echo 'alias ls="ls --color=auto"'; \
	} > /root/.bashrc \
	&& chmod +x /root/.bashrc; \
	# 下载sgerrand的公钥文件.
	wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub; \
    # 下载glibc glibc-bin glibc-i18n:
    success=; \
    for url in $GLIBC_TGZ_URLS; do \
        if wget -O /tmp/glibc-2.31-r0.apk "$url"; then \
            success=1; \
            break; \
        fi; \
    done; \
    for url in $GLIBC_BIN_URLS; do \
        if wget -O /tmp/glibc-bin-2.31-r0.apk "$url"; then \
            success=1; \
            break; \
        fi; \
    done; \
    for url in $GLIBC_I18N_URLS; do \
        if wget -O /tmp/glibc-i18n-2.31-r0.apk "$url"; then \
            success=1; \
            break; \
        fi; \
    done; \
    [ -n "$success" ]; \
    # 安装glibc,删除安装包
    apk add --allow-untrusted /tmp/glibc-2.31-r0.apk /tmp/glibc-bin-2.31-r0.apk /tmp/glibc-i18n-2.31-r0.apk; \
    echo 'hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4' >> /etc/nsswitch.conf; \
	rm -f /tmp/glibc-2.31-r0.apk /tmp/glibc-bin-2.31-r0.apk /tmp/glibc-i18n-2.31-r0.apk; \
	rm -rf /var/cache/apk/* ; \
	# 处理glibc对多语言系统的支持. 仅添加了用得到的一部分语言,其他的语言暂且先注释掉
	{ \
#	     echo 'aa_DJ'; \
#        echo 'aa_ER'; \
#        echo 'aa_ET'; \
#        echo 'af_ZA'; \
#        echo 'am_ET'; \
#        echo 'an_ES'; \
#        echo 'ar_AE'; \
#        echo 'ar_BH'; \
#        echo 'ar_DZ'; \
#        echo 'ar_EG'; \
#        echo 'ar_IN'; \
#        echo 'ar_IQ'; \
#        echo 'ar_JO'; \
#        echo 'ar_KW'; \
#        echo 'ar_LB'; \
#        echo 'ar_LY'; \
#        echo 'ar_MA'; \
#        echo 'ar_OM'; \
#        echo 'ar_QA'; \
#        echo 'ar_SA'; \
#        echo 'ar_SD'; \
#        echo 'ar_SY'; \
#        echo 'ar_TN'; \
#        echo 'ar_YE'; \
#        echo 'as_IN'; \
#        echo 'ast_ES'; \
#        echo 'ayc_PE'; \
#        echo 'az_AZ'; \
#        echo 'be_BY'; \
#        echo 'bem_ZM'; \
#        echo 'ber_DZ'; \
#        echo 'ber_MA'; \
#        echo 'bg_BG'; \
#        echo 'bho_IN'; \
#        echo 'bn_BD'; \
#        echo 'bn_IN'; \
#        echo 'bo_CN'; \
#        echo 'bo_IN'; \
#        echo 'br_FR'; \
#        echo 'brx_IN'; \
#        echo 'bs_BA'; \
#        echo 'byn_ER'; \
#        echo 'ca_AD'; \
#        echo 'ca_ES'; \
#        echo 'ca_FR'; \
#        echo 'ca_IT'; \
#        echo 'crh_UA'; \
#        echo 'csb_PL'; \
#        echo 'cs_CZ'; \
#        echo 'cv_RU'; \
#        echo 'cy_GB'; \
        echo 'da_DK'; \
        echo 'de_AT'; \
        echo 'de_BE'; \
        echo 'de_CH'; \
        echo 'de_DE'; \
        echo 'de_LU'; \
#        echo 'doi_IN'; \
#        echo 'dv_MV'; \
#        echo 'dz_BT'; \
#        echo 'el_CY'; \
#        echo 'el_GR'; \
        echo 'en_AG'; \
        echo 'en_AU'; \
        echo 'en_BW'; \
        echo 'en_CA'; \
        echo 'en_DK'; \
        echo 'en_GB'; \
        echo 'en_HK'; \
        echo 'en_IE'; \
        echo 'en_IN'; \
        echo 'en_NG'; \
        echo 'en_NZ'; \
        echo 'en_PH'; \
        echo 'en_SG'; \
        echo 'en_US'; \
        echo 'en_ZA'; \
        echo 'en_ZM'; \
        echo 'en_ZW'; \
#        echo 'es_AR'; \
#        echo 'es_BO'; \
#        echo 'es_CL'; \
#        echo 'es_CO'; \
#        echo 'es_CR'; \
#        echo 'es_CU'; \
#        echo 'es_DO'; \
#        echo 'es_EC'; \
#        echo 'es_ES'; \
#        echo 'es_GT'; \
#        echo 'es_HN'; \
#        echo 'es_MX'; \
#        echo 'es_NI'; \
#        echo 'es_PA'; \
#        echo 'es_PE'; \
#        echo 'es_PR'; \
#        echo 'es_PY'; \
#        echo 'es_SV'; \
#        echo 'es_US'; \
#        echo 'es_UY'; \
#        echo 'es_VE'; \
#        echo 'et_EE'; \
        echo 'eu_ES'; \
#        echo 'fa_IR'; \
#        echo 'ff_SN'; \
#        echo 'fi_FI'; \
#        echo 'fil_PH'; \
#        echo 'fo_FO'; \
        echo 'fr_BE'; \
        echo 'fr_CA'; \
        echo 'fr_CH'; \
        echo 'fr_FR'; \
        echo 'fr_LU'; \
        echo 'fur_IT'; \
#        echo 'fy_DE'; \
#        echo 'fy_NL'; \
#        echo 'ga_IE'; \
#        echo 'gd_GB'; \
#        echo 'gez_ER'; \
#        echo 'gez_ET'; \
#        echo 'gl_ES'; \
#        echo 'gu_IN'; \
#        echo 'gv_GB'; \
#        echo 'ha_NG'; \
#        echo 'he_IL'; \
#        echo 'hi_IN'; \
#        echo 'hne_IN'; \
#        echo 'hr_HR'; \
#        echo 'hsb_DE'; \
#        echo 'ht_HT'; \
#        echo 'hu_HU'; \
#        echo 'hy_AM'; \
#        echo 'ia_FR'; \
#        echo 'id_ID'; \
#        echo 'ig_NG'; \
#        echo 'ik_CA'; \
#        echo 'is_IS'; \
#        echo 'it_CH'; \
#        echo 'it_IT'; \
#        echo 'iu_CA'; \
        echo 'ja_JP'; \
#        echo 'ka_GE'; \
#        echo 'kk_KZ'; \
#        echo 'kl_GL'; \
#        echo 'km_KH'; \
#        echo 'kn_IN'; \
        echo 'kok_IN'; \
        echo 'ko_KR'; \
#        echo 'ks_IN'; \
#        echo 'ku_TR'; \
#        echo 'kw_GB'; \
#        echo 'ky_KG'; \
#        echo 'lb_LU'; \
#        echo 'lg_UG'; \
#        echo 'li_BE'; \
#        echo 'lij_IT'; \
#        echo 'li_NL'; \
#        echo 'lo_LA'; \
#        echo 'lt_LT'; \
#        echo 'lv_LV'; \
#        echo 'mag_IN'; \
#        echo 'mai_IN'; \
#        echo 'mg_MG'; \
#        echo 'mhr_RU'; \
#        echo 'mi_NZ'; \
#        echo 'mk_MK'; \
#        echo 'ml_IN'; \
#        echo 'mni_IN'; \
#        echo 'mn_MN'; \
#        echo 'mr_IN'; \
#        echo 'ms_MY'; \
#        echo 'mt_MT'; \
#        echo 'my_MM'; \
#        echo 'nb_NO'; \
#        echo 'nds_DE'; \
#        echo 'nds_NL'; \
#        echo 'ne_NP'; \
#        echo 'nhn_MX'; \
#        echo 'niu_NU'; \
#        echo 'niu_NZ'; \
#        echo 'nl_AW'; \
#        echo 'nl_BE'; \
#        echo 'nl_NL'; \
#        echo 'nn_NO'; \
#        echo 'nr_ZA'; \
#        echo 'nso_ZA'; \
#        echo 'oc_FR'; \
#        echo 'om_ET'; \
#        echo 'om_KE'; \
#        echo 'or_IN'; \
#        echo 'os_RU'; \
#        echo 'pa_IN'; \
#        echo 'pa_PK'; \
#        echo 'pl_PL'; \
#        echo 'ps_AF'; \
#        echo 'pt_BR'; \
#        echo 'pt_PT'; \
#        echo 'ro_RO'; \
        echo 'ru_RU'; \
        echo 'ru_UA'; \
#        echo 'rw_RW'; \
#        echo 'sa_IN'; \
#        echo 'sat_IN'; \
#        echo 'sc_IT'; \
#        echo 'sd_IN'; \
#        echo 'se_NO'; \
#        echo 'shs_CA'; \
#        echo 'sid_ET'; \
#        echo 'si_LK'; \
#        echo 'sk_SK'; \
#        echo 'sl_SI'; \
#        echo 'so_DJ'; \
#        echo 'so_ET'; \
#        echo 'so_KE'; \
#        echo 'so_SO'; \
#        echo 'sq_AL'; \
#        echo 'sq_MK'; \
#        echo 'sr_ME'; \
#        echo 'sr_RS'; \
#        echo 'ss_ZA'; \
#        echo 'st_ZA'; \
#        echo 'sv_FI'; \
#        echo 'sv_SE'; \
#        echo 'sw_KE'; \
#        echo 'sw_TZ'; \
#        echo 'szl_PL'; \
#        echo 'ta_IN'; \
#        echo 'ta_LK'; \
#        echo 'te_IN'; \
#        echo 'tg_TJ'; \
#        echo 'th_TH'; \
#        echo 'ti_ER'; \
#        echo 'ti_ET'; \
#        echo 'tig_ER'; \
#        echo 'tk_TM'; \
#        echo 'tl_PH'; \
#        echo 'tn_ZA'; \
#        echo 'tr_CY'; \
#        echo 'tr_TR'; \
#        echo 'ts_ZA'; \
#        echo 'tt_RU'; \
#        echo 'ug_CN'; \
#        echo 'uk_UA'; \
#        echo 'unm_US'; \
#        echo 'ur_IN'; \
#        echo 'ur_PK'; \
#        echo 'uz_UZ'; \
#        echo 've_ZA'; \
#        echo 'vi_VN'; \
#        echo 'wa_BE'; \
#        echo 'wae_CH'; \
#        echo 'wal_ET'; \
#        echo 'wo_SN'; \
#        echo 'xh_ZA'; \
#        echo 'yi_US'; \
#        echo 'yo_NG'; \
#        echo 'yue_HK'; \
        echo 'zh_CN'; \
        echo 'zh_HK'; \
        echo 'zh_SG'; \
        echo 'zh_TW'; \
        echo 'zu_ZA'; \
	} > /locale.md; \
	cat locale.md | xargs -i /usr/glibc-compat/bin/localedef -i {} -f UTF-8 {}.UTF-8;

ENV PATH ${PATH}:/usr/glibc-compat/bin

CMD ["/bin/bash"]